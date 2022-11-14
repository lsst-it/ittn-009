..
  Technote content.

  See https://developer.lsst.io/restructuredtext/style.html
  for a guide to reStructuredText writing.

  Do not put the title, authors or other metadata in this document;
  those are automatically added.

  Use the following syntax for sections:

  Sections
  ========

  and

  Subsections
  -----------

  and

  Subsubsections
  ^^^^^^^^^^^^^^

  To add images, add the image file (png, svg or jpeg preferred) to the
  _static/ directory. The reST syntax for adding the image is

  .. figure:: /_static/filename.ext
     :name: fig-label

     Caption text.

   Run: ``make html`` and ``open _build/html/index.html`` to preview your work.
   See the README at https://github.com/lsst-sqre/lsst-technote-bootstrap or
   this repo's README for more info.

   Feel free to delete this instructional comment.

:tocdepth: 1

.. Please do not modify tocdepth; will be fixed when a new Sphinx theme is shipped.

.. sectnum::

.. TODO: Delete the note below before merging new content to the master branch.

.. Add content here.
.. Do not include the document title (it's automatically added from metadata.yaml).

Linux System Clock
==================

The Linux kernel handles the system clock in `Unix time
<https://en.wikipedia.org/wiki/Unix_time>`_. ``Unix time``, which is a
monotonic count of seconds since the epoch of 1970-01-01 00:00:00 UTC.  The
system clock is initially set from a hardware real time clock (RTC) when the
system is booted. The system clock and RTC are both defined to be UTC and there
is no facility for instructing the kernel that an alternative epoch is in use.
The system clock is the definitive source of time on the system.  While the
kernel does support obtaining timestamps in TAI via system calls, TAI time
is always computed as an offset from the system clock.

While it is possible to set the system clock to be synchronous with ``TAI``
time without the kernel's knowledge, this may cause a number of issues,
including:

- UTC leap second corrections mistakenly being applied to the system clock as if it is UTC time
- The timestamps in log messages being offset from UTC without any indication
  that said timestamps are not in UTC
- Interoperability issues with `kerberos
  <https://en.wikipedia.org/wiki/Kerberos_(protocol)>`_ (krb5) ticket-granting tickets
  (TGT), which rely on timestamps in UTC. Rubin Observatory uses krb5 for system authz.
- Interoperability issues with `x509 <https://en.wikipedia.org/wiki/X.509>`_
  certificates, which use UTC timestamnp to establish a validity period.  Rubin
  Observatory uses some management tooling such as puppet which, which is
  dependent upon x509 certs.
- Applications that sanity check timestamps to ensure that UTC != TAI will
  fail.

Leap Seconds
============

Earth's rotational period is not exactly 86400 seconds, which causes the
time of day to gradually slip earlier. To compensate for this an extra second
is periodically added (or subtracted) from from UTC to keep the delta between
UTC and UT1 under 1 second.

However, leap seconds may cause the clock to behave in ways that many
applications don't expect. One such quirk is that days that have a leap second
have a minute that either 59 or 61 seconds long.  One possible issue is the
expectation that timestamps "seconds" are in the range 00-59. E.g.:

.. code-block:: bash

   $ TZ=right/UTC date -d 'Dec 31 2008 23:59:60'
   Wed Dec 31 23:59:60 UTC 2008

As leap seconds are a relatively infrequent event, and likely due to low
developer awareness, leap second handling problems in applications are often
unknown/undetected. In order to avoid triggering latent software bugs, it has
become reasonably common to smear/spread/slew the leap second across a larger
time period. Typically, this is a day and over the course of that day each
"clock second" is slight more or less than an SI second.  This avoids ever
having a timestamp of ``23:59:60`` or skipping over second ``23:59:59`` and
avoids sudden clock shifts.  However, this strategy inherently relies on
intentionally making the system clock subtly inaccurate.

CLOCK_TAI: The short story
==========================

#. Linux calculates all kernel clocks by reading ``CLOCK_MONOTONIC`` and adding offsets. There is only one actual clock; all others are synthetic.
#. By default Linux sets ``CLOCK_TAI`` to match ``CLOCK_REALTIME`` on boot.
#. Applying the correct UTC/TAI offset to ``CLOCK_TAI`` must be done with an application like ``ntpd``, ``chrony``, or ``linuxptp``.
#. ``CLOCK_TAI`` pushes the responsibility of dealing with leap seconds, leap second smearing, and other time offset issues into the Linux kernel and  time synchronization daemons.
#. It is extremely difficult to timestamp events with precision in the domain of 50ns-5us because Linux does not provide realtime guarantees.

CLOCK_TAI: The long story
=========================

On Linux, ``CLOCK_TAI`` is not an independent timer; rather it (along with all
other clocks) are defined by offsets from the Linux monotonic clock.

CLOCK_TAI kernel clock implementation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We first start by looking at the definition of the ``CLOCK_TAI`` clock.

https://github.com/torvalds/linux/blob/v5.5/kernel/time/posix-timers.c#L1311-L1325

.. code-block:: c

   static const struct k_clock clock_tai = {
        .clock_getres        = posix_get_hrtimer_res,
        .clock_get           = posix_get_tai,
        .nsleep              = common_nsleep,
        .timer_create        = common_timer_create,
        .timer_set           = common_timer_set,
        .timer_get           = common_timer_get,
        .timer_del           = common_timer_del,
        .timer_rearm         = common_hrtimer_rearm,
        .timer_forward       = common_hrtimer_forward,
        .timer_remaining     = common_hrtimer_remaining,
        .timer_try_to_cancel = common_hrtimer_try_to_cancel,
        .timer_wait_running  = common_timer_wait_running,
        .timer_arm           = common_hrtimer_arm,
   };

This leads us to the ``posix_get_tai`` function.

https://github.com/torvalds/linux/blob/v5.5/kernel/time/posix-timers.c#L231-L235

.. code-block:: c

   static int posix_get_tai(clockid_t which_clock, struct timespec64 *tp)
   {
           ktime_get_clocktai_ts64(tp);
           return 0;
   }

https://github.com/torvalds/linux/blob/v5.5/include/linux/timekeeping.h#L202-L205

.. code-block:: c

   static inline void ktime_get_clocktai_ts64(struct timespec64 *ts)
   {
           *ts = ktime_to_timespec64(ktime_get_clocktai());
   }


https://github.com/torvalds/linux/blob/v5.5/include/linux/timekeeping.h#L103-L109

.. code-block:: c

   /**
    * ktime_get_clocktai - Returns the TAI time of day in ktime_t format
    */
   static inline ktime_t ktime_get_clocktai(void)
   {
           return ktime_get_with_offset(TK_OFFS_TAI);
   }

This leads us to the ``ktime_get_with_offset`` function, which reads the
monotonic clock and calculates offsets from that clock to determine the value
of other clocks (``CLOCK_TAI``, ``CLOCK_REALTIME``, ``CLOCK_BOOTIME``, etc.)

https://github.com/torvalds/linux/blob/v5.5/kernel/time/timekeeping.c#L790-L808

.. code-block:: c

   ktime_t ktime_get_with_offset(enum tk_offsets offs)
   {
           struct timekeeper *tk = &tk_core.timekeeper;
           unsigned int seq;
           ktime_t base, *offset = offsets[offs];
           u64 nsecs;

           WARN_ON(timekeeping_suspended);

           do {
                   seq = read_seqcount_begin(&tk_core.seq);
                   base = ktime_add(tk->tkr_mono.base, *offset);
                   nsecs = timekeeping_get_ns(&tk->tkr_mono);

           } while (read_seqcount_retry(&tk_core.seq, seq));

           return ktime_add_ns(base, nsecs);

   }

We can see that the ``CLOCK_REALTIME``, ``CLOCK_BOOTTIME``, and ``CLOCK_TAI``
are offsets.

https://github.com/torvalds/linux/blob/v5.5/kernel/time/timekeeping.c#L784-L788

.. code-block:: c

   static ktime_t *offsets[TK_OFFS_MAX] = {
           [TK_OFFS_REAL] = &tk_core.timekeeper.offs_real,
           [TK_OFFS_BOOT] = &tk_core.timekeeper.offs_boot,
           [TK_OFFS_TAI]  = &tk_core.timekeeper.offs_tai,
   };

Timestamping with vDSO
^^^^^^^^^^^^^^^^^^^^^^

We can also look at how vDSO provides user space access to the current time. In
this example we're taking the offset between the coarse monotonic clock
(``CS_HRES_COARSE``) and the atomic clock.

https://github.com/torvalds/linux/blob/v5.5/kernel/time/vsyscall.c#L69-L72

.. code-block:: c

   static inline void update_vdso_data(struct vdso_data *vdata,
                                       struct timekeeper *tk)
   {
           // [...]

           /* CLOCK_TAI */
           vdso_ts              = &vdata[CS_HRES_COARSE].basetime[CLOCK_TAI];
           vdso_ts->sec         = tk->xtime_sec + (s64)tk->tai_offset;
           vdso_ts->nsec        = tk->tkr_mono.xtime_nsec;

           // [...]
   }

.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
