:tocdepth: 1

.. sectnum::

*“We must use time as a tool, not as a couch.”* – John F. Kennedy

Terminology
===========

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this
document are to be interpreted as described in `BCP 14
<https://www.rfc-editor.org/info/bcp14>`_ [`RFC2119
<https://datatracker.ietf.org/doc/html/rfc2119>`_] [`RFC8174
<https://datatracker.ietf.org/doc/html/rfc8174>`_] when, and only when, they
appear in all capitals, as shown here.

Requirements Flow-down
======================

LSE-030 2.6.1 System Time Reference
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``OSS-REQ-0086``

*Specficiation: The LSST system shall provide an observatory wide standard time reference
that shall be used by all subsystems where absolute and external time reference is required*

LSE-030 2.6.1.1 Time Accuracy and Precision
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``OSS-REQ-0087``

*Specficiation: Computer clocks used to produce timestamps shall be synchronized with an
observatory master clock to a precision of timestampPrecision and an accuracy of
timestampAccuracy, as given in the table below. This requirement shall apply separately to
each computer clock.*

.. list-table::
   :header-rows: 1

   * - Description
     - Value
     - Unit
     - Name
   * - Computer clock timestamp accuracy
     - 0.010
     - second
     - timestampAccuracy
   * - Computer clock timestamp precision
     - 0.001
     - second
     - timestampPrecision

LSE-030 2.6.1.2 Time Reporting Standard
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``OSS-REQ-0089``

*Specification: The time reporting standard shall be International Atomic Time (TAI).*

LSE-59 7.4.1 Timestamp Accuracy and Precision
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``CAM-REQ-0111``

*Specification: Computer clocks used to produce timestamps shall be synchronized with an
observatory master clock to a precision of [timestampPrecision] and an accuracy of
[timestampAccuracy], as given in the table below. This requirement shall apply separately
to each computer clock.*

.. list-table::
   :header-rows: 1

   * - Description
     - Value
     - Unit
     - Name
   * - Computer clock timestamp accuracy
     - 0.010
     - second
     - timestampAccuracy
   * - Computer clock timestamp precision
     - 0.001
     - second
     - timestampPrecision

LSE-60 6.1 Telescope Time Reference
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``TLS-REQ-0138``

*Specification: The Telescope and Site system shall provide a standard time
reference to be used by all for absolute and external time reference.*

Derived from requirements:

- ``OSS-REQ-0086``: System Time Reference

LSE-60 6.1.1 Time Absolute Accuracy and Relative Precision
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``TLS-REQ-0139``

*Specification: All time tagged events reported both internally and externally
by the Telescope and Site shall be done with the timing Absolute_Accuracy and
Relative_Precision given in the table below.*

.. list-table::
   :header-rows: 1

   * - Description
     - Value
     - Unit
     - Name
   * - All time tagged events reported both internally and externally shall be done with an accuracy of Absolute_Accuracy.
     - 0.010
     - second
     - Absolute_Accuracy
   * - All internal events shall be recorded with a precision relative to the master clock of Relative_Precision.
     - 0.001
     - second
     - Relative_Precision

Derived from requirements:

- ``OSS-REQ-0087``: Time Accuracy and Precision

LSE-60 6.1.2 Telescope Internal Time Standard
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``TLS-REQ-0140``

*Specification: The Telescope and Site internal time reporting standard shall be
International Atomic Time (TAI).*

Derived from requirements:

- ``OSS-REQ-0089``: Time Reporting Standard

LSE-61 1.2.3 Raw Science Image Metadata
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``DMS-REQ-0068 (Priority: 1a)``

*Specification: For each raw science image, the DMS shall store image metadata including at
least:
- Time of exposure start and end, referenced to TAI, and DUT1
...*

LSE-70 3.1.3 PTP Clock Synchronization Protocol
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``SYS-ALL-COM-ICD-0040``

*Specification: The Rubin Observatory components shall follow the PTP version 2 protocol
when requiring time accuracy better than 1 millisecond.*

LSE-70 3.1.4 NTP Clock Synchronization Protocol
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``SYS-ALL-COM-ICD-0059``

*Specification: The Rubin Observatory components shall follow the NTP version 4 protocol
when requiring time accuracy no better than 1 millisecond.*

LSE-70 3.2.4 TAI alignment
^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``SYS-ALL-COM-ICD-0055``

*Specification: The SAL middleware shall align the internal time to the TAI system clock.*

LSE-70 3.2.5 UTC leap second
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``SYS-ALL-COM-ICD-0056``

*Specification: The SAL middleware shall provide to the CSC the UTC leap second offset
between TAI and UTC upon request of the CSC.*

LTS-97 3.18.15 DCS TIME
^^^^^^^^^^^^^^^^^^^^^^^

**ID:** NONE

*Specification: The DCS shall utilize the time provided by the LSST summit
network using Precision Time Protocol (PTP) IEEE 1588-2008 or later. The
systems do not need to be directly synchronized with each other; instead they
are using the same master clock.*

.. note::

  **Does the DCS require sub-millisecond timing accuracy?**

LTS-103 3.10.11 Time
^^^^^^^^^^^^^^^^^^^^

**ID:** NONE

*Specification: The MCS shall utilize the time provided by the LSST summit
network using Precision Time Protocol (PTP) IEEE 1588-2008 or later. The
systems do not need to be directly synchronized with each other; instead they
are using the same time.*

.. note::

  **Does the MCS require sub-millisecond timing accuracy?**

LTS-158 5.1 DCS telemetry time-stamp
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``LTS-158-REQ-0004``

*Specification: The DCS shall timestamp all published telemetry.*

.. note::

  **Perhaps this requirement should reference ``OSS-REQ-0086`` and ``OSS-REQ-0087``?**

LTS-159 5.1 Mount Telemetry Time-stamp
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``LTS-159-REQ-0004``

*Specification: The Mount shall time-stamp all published telemetry topics.*

.. note::

  **Perhaps this requirement should reference ``OSS-REQ-0086`` and ``OSS-REQ-0087``?**

LTS-160 5.1 Hexapods and Rotator Telemetry Time-stamp
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``LTS-160-REQ-0004``

*Specification: The Hexapod and Rotator SHALL time-stamp all published telemetry topics.*

.. note::

  **Perhaps this requirement should reference ``OSS-REQ-0086`` and ``OSS-REQ-0087``?**

LTS-162 3.2 M2 Assembly telemetry time-stamp
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``LTS-162-REQ-0005``

*Specification: The M2 Assembly shall time-stamp all published telemetry.*

.. note::

  **Perhaps this requirement should reference ``OSS-REQ-0086`` and ``OSS-REQ-0087``?**

LTS-206 3.7.1.6 Time
^^^^^^^^^^^^^^^^^^^^

**ID:** LTS-206-REQ-0209

*Specification: The Control System shall utilize the time provided by the LSST
summit network.*

Derived from requirements:

- ``TLS-REQ-0138``: Telescope Time Reference
- ``TLS-REQ-0139``: Time Absolute Accuracy and Relative Precision
- ``TLS-REQ-0140``: Telescope Internal Time Standar

LTS-306 2.5.3.3 Follow Clock Synchronization Protocol
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``SYS-ALL-COM-ICD-0012``

**Specification: System components requiring accurate time shall follow the IEEE
1588-2008 Standard for a Precision Clock Synchronization Protocol for Networked
Measurement and Control Systems, also known as PTP Version 2.**

LTS-306 2.5.3.6 Interpret internal time in displayed timestamp
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** ``SYS-ALL-COM-ICD-0015``

**Specification: The system shall convert PTP time to UTC upon request.
Discussion: PTP time (internal representation) uses TAI (elapsed time from
reference date--no leap seconds), but UTC uses leap seconds.**

.. note::

  **This requirement may have been assuming that the system clock would be set
  to TAI because PTP was used as the syncronnization mechanism? This may not
  make sense with the system clock set to UTC.**


LTS-440 2.2 Hardware Communication and Telemetry
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**ID:** **Missing?**

*Specification: Wherever possible, hardware selection shall be compatible with a
National Instruments Compact Rios Device. The software used to control hardware
devices shall be compatible with 64-bit Linux CentOS 7. The LSST-CBP system
shall be equipped with a PTP capable network card to provide time services
(Intel i210 is the current default recommendation).*

.. note::

  **Requirement ID/tags appears to be missing from this document.**

Specification
=============

System Clock
^^^^^^^^^^^^

The system clock of all Linux hosts SHALL be synchronized to UTC. Similarly,
the real-time clock (RTC) of all Linux hosts SHALL be synchronized to the
system clock in UTC.

System timestamps in UTC and TAI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

All Linux hosts, regardless of the time synchronization protocol used, SHALL
set the kernel's internal UTC-TAI offset.

Linux hosts SHALL be configured such that the `clock_gettime(2)
<https://man7.org/linux/man-pages/man3/clock_gettime.3.html>`_ system call will
provide a timestamp that conforms to ``timestampAccuracy`` and
``timestampPrecision`` when called with the following values for ``clock_id``.:

- ``CLOCK_REALTIME``
- ``CLOCK_TAI``

Relevent requirements
"""""""""""""""""""""

- ``OSS-REQ-0087``
- ``OSS-REQ-0089``
- ``DMS-REQ-0068 (Priority: 1a)``

Network Time Protocol
^^^^^^^^^^^^^^^^^^^^^^

`Network Time Protocol (NTP)
<https://en.wikipedia.org/wiki/Network_Time_Protocol>`_ synchronization is
generally considered capable of accuracy better than 1ms with a stratum 1 time
source access via a local area network.  This easily exceeds the
``timestampAccuracy`` requirement. NTP data includes the UTC-TAI offset and
information about upcoming leap seconds. NTP clients provide excellent
resiliency, and even slight tolerance of misbehaving timesource(s), due to the
ability to simultaneously work with multiple authoritative time sources. No
traceable requirement establishes a need for time synchronization accuracy
greater than +/- 10 milliseconds, which is easily achived with NTP.

- NTP SHALL be considered the default time synchronization method for hosts at
  the summit, unless a subsystem has explicitly opted to use PTP.
- At least 3 stratrum 1 NTP clocks with GPS receivers SHALL be present at the
  summit.
- Enterprise Linux (EL) hosts using NTP SHALL be configured as a client of at
  least *local* 3 stratum 1 NTP clocks.
- Enterprise Linux (EL) hosts using NTP MAY have additional *remote* stratrum 1
  time sources configured.
- `Chrony <https://chrony.tuxfamily.org/>`_ SHALL be the NTP client software
  used on condition the system clock on EL hosts.
- NTP client software other than ``chrony`` MAY be used on embedded devices or
  Linux distributions outside of the EL family. ``chrony`` is the RECOMMENDED
  solution for NTP sync on all platforms which it is readily amiable.
- ``chrony`` SHALL be configured with ``leapsectz right/UTC`` to enable setting
  the kernel's UTC-TAI offset.
- ``chrony`` SHALL be configured with ``leapsecmode system`` to enable the
  kernel to handle leap second transitions.

Relevent requirements
"""""""""""""""""""""

- ``OSS-REQ-0086``

Precision Time Protocol
^^^^^^^^^^^^^^^^^^^^^^^

`Precision Time Protoocol (PTP)
<https://en.wikipedia.org/wiki/Precision_Time_Protocol>`_ is capable of
sub-microsecond absolute time accuracy. However, PTP is less resilient than NTP
for general purpose hosts as there may only be one master clock at a time on a
network. PTP also has increased administrative over head NTP due to requiring
support both by network switches and special hardware requirements for the
network interface card (NIC) used for PTP synchronization. PTP has more "moving
parts" and thus more failure modes than NTP.  PTP data does include the TAI-UTC
offset. The use of PTP is NOT RECOMMENDED when NTP's capabilities are
sufficient (which appear to be true based on the identified traceable
requirements). This is due to the installation base of hosts using NTP likely
being *at least* 3 orders of magnitude greater than that of PTP. For linux
systems, there were two primary PTP implementations: `ptpd
<https://github.com/ptpd/ptpd>`_ and `linuxptp
<https://linuxptp.sourceforge.net/>`_.  It appears that ``ptpd`` has been
unsupported since 2019.  ``linuxptp`` has recent code commits from Sept. 2022
and is likely only viable solution for PTP support on Linux.  However, real
world experience has encountered problems with ``linuxptp``.  It is assumed
that hosts using ``linuxptp`` are more likley to experience system clock
synchronization problems.

- A primary grandmaster PTP clocks with GPS receivers SHALL be present at the
  summit.
- A backup grandmaster PTP clock with GPS receivers SHALL be present at the
  summit.
- Subsystems MAY elect to "opt-in" a host in to using PTP instead of NTP.
- Only PTP version 2 SHALL be supported.
- PTP SHALL only be supported when the device is connected directly to a network switch which capable of, and has been, configured as a PTP boundary clock.
- PTP SHALL only be supported on hosts with a NIC with a PHC
- `ptp4l <https://linuxptp.sourceforge.net/>`_ SHALL be used to synchronize the PHC to PTP
- ``chrony`` SHALL be used to synchronize the system clock with the PHC.
- PTP SHALL only be supported on operating systems in the EL family.

.. note::

   **TBD: Does automatic UTC-TAI offset work for ptp4l + chrony? Or Does the
   offset have to be manually set, and thus cause UTC times to be in error by 1
   second when a new leap second is injected? phc2sys may be needed instead of
   chrony.**

Relevent requirements
"""""""""""""""""""""

- ``OSS-REQ-0086``

Reference Information
=====================

Linux System Clock
^^^^^^^^^^^^^^^^^^

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
^^^^^^^^^^^^

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
^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Linux calculates all kernel clocks by reading ``CLOCK_MONOTONIC`` and adding offsets. There is only one actual clock; all others are synthetic.
#. By default Linux sets ``CLOCK_TAI`` to match ``CLOCK_REALTIME`` on boot.
#. Applying the correct UTC/TAI offset to ``CLOCK_TAI`` must be done with an application like ``ntpd``, ``chrony``, or ``linuxptp``.
#. ``CLOCK_TAI`` pushes the responsibility of dealing with leap seconds, leap second smearing, and other time offset issues into the Linux kernel and  time synchronization daemons.
#. It is extremely difficult to timestamp events with precision in the domain of 50ns-5us because Linux does not provide realtime guarantees.

CLOCK_TAI: The long story
^^^^^^^^^^^^^^^^^^^^^^^^^

On Linux, ``CLOCK_TAI`` is not an independent timer; rather it (along with all
other clocks) are defined by offsets from the Linux monotonic clock.

CLOCK_TAI kernel clock implementation
"""""""""""""""""""""""""""""""""""""

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
""""""""""""""""""""""

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

Example Chrony NTP Configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: unixconfig

   # This file is being maintained by Puppet. Do not edit.

   # NTP servers
   server 140.252.1.140 iburst
   server 140.252.1.141 iburst
   server 140.252.1.142 iburst

   # Record the rate at which the system clock gains/losses time.
   driftfile /var/lib/chrony/drift

   # Enable kernel RTC synchronization.
   rtcsync

   # In first 3 updates step the system clock instead of slew
   # if the adjustment is larger than 10 seconds.
   makestep 10 3

   bindcmdaddress 127.0.0.1
   bindcmdaddress ::1

   # Serve time even if not synchronized to any NTP server.
   local stratum 10

   keyfile /etc/chrony.keys

   # Disable logging of client accesses.
   noclientlog

   # Send a message to syslog if a clock adjustment is larger than the specified threshold
   logchange 0.5

   logdir /var/log/chrony

   # https://chrony.tuxfamily.org/doc/3.4/chrony.conf.html#leapsecmode
   leapsecmode system

   # https://chrony.tuxfamily.org/doc/3.4/chrony.conf.html#leapsectz
   leapsectz right/UTC

.. .. rubric:: References

.. Make in-text citations with: :cite:`bibkey`.

.. .. bibliography:: local.bib lsstbib/books.bib lsstbib/lsst.bib lsstbib/lsst-dm.bib lsstbib/refs.bib lsstbib/refs_ads.bib
..    :style: lsst_aa
