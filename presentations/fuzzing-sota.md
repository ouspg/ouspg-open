# Fuzzing state of the art

*by 21 mostly anonymous workshop participants (including ...)*

Fuzzing is about breaking contracts, rules, boundaries, software and systems.

Fuzzing has helped us to break things in order to learn what to fix or how to build things better.

## Fuzzing as exploratory testing (where it all started from)

 * look for new types of issues / hunt for bugs

## Fuzzing as test automation / continuous testing (late trend and continuum)

 * set up once, look for regressions
 * Fuzzing as part of normal QA processes
    - q: suitability due to determinism/repeatability in CI or acceptance testing

## Approaches

 * evolutionary
  * other feedback driven
 * zero case: generate all possible inputs in order
 * cat /dev/random > (only one that can find everything - [within certain "theoretical" limits](https://en.wikipedia.org/wiki/Limits_to_computation))
  * Ultimate limit of Life, Universe and Fuzzing = 42?
 * mutational
 * white-box
 * black-box
 * sample based
 * protocol specification (model) based

## Related approaches

 * Sources to help to build models?
 * Sources to help in instrumentation?
 * concolic/symbolic execution (or is this non-fuzzing?)
  * see Driller tool below
  * mixed fuzzing with symbolic execution
  * [Symbolic Execution for Software Testing in Practice - Preliminary Assessment](http://research.microsoft.com/en-us/um/people/pg/public_psfiles/icse2011.pdf)
 * Fuzzing vs. intermediate languages (CLR/LLVM/...)
 * reversing binary applications to pinpoint potential problem areas
 * statistical approaches to analyze code / target fuzzing effort

## Instrumentation?

 * for deviations from accepted/safe state (did we find something?)
 * for failure analysis (what did we find and was this unique?)
 * for improving code & state space coverage (can we fuzz better?)
 * coverage
 * CLANG (something similar for gcc)
  * [ASAN](http://clang.llvm.org/docs/AddressSanitizer.html)
  * [MSAN](http://clang.llvm.org/docs/MemorySanitizer.html)
  * [UBSAN](http://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html)
  * [DFSAN](http://clang.llvm.org/docs/DataFlowSanitizer.html)
  * [LeakSanitizer](http://clang.llvm.org/docs/LeakSanitizer.html)
  * [TSAN](http://clang.llvm.org/docs/ThreadSanitizer.html)
 * OpenBSD's pledge() and malloc.conf
 * valgrind
 * Valid case
 * CCM (Control Capability Monitoring, ISA secure)
  * monitors that the target performs its critical tasks acceptably under duress
  * SLA instrumentation
 * SNMP
 * Syslog
 * Custom instrumentation
  * Fuzz web service -> figure out funny stuff in database
  * System level instrumentation
   * Status of the ECU's in automotive / CAN fuzzing
   * Status of PLC's in ICS
   * ...
 * PIN (also, http://shell-storm.org/blog/In-Memory-fuzzing-with-Pin/)

## Who does it?

 * Bounty hunters
 * Academics (sudden flood of fuzzing papers)
 * Clueful software vendors (commercial or open source) / Top notch vendors?
  * Most prominent (in-house fuzzing developers)
   * Google (most open, most coverage)
   * Mozilla (open, targets their own)
   * Microsoft (they do it, not that open)
  * Cisco
  * ...
 * Criminals and/or ...
 * State-level
  * Product evaluation / accreditation / approval
  * military / intelligence
 * any-hat?
  * Sysadmins?
  * Pen testers?
  * Tiger teams?
 * increased adoption in correctness, non-security contexts (eg. among SAT solver hackers)
  * [QuickCheck](https://en.wikipedia.org/wiki/QuickCheck) has been long used like this
   * x == decode(encode(x)) for any valid x

## Standards / certifications / "best practices"

 * IEC 61850 for medical/ICS
 * Defines what to fuzz, not how
 * ISAsecure
  * More tradional compliance testing
 * Underwriters Laboratory's UL CAP (Cybersecurity Assurance Program) / UL 2900 series
 * Various SDLC:s state that fuzzing should be performed
   * So do various software approval schemes (e.g. [CESG Commercial Product Assurance](https://www.cesg.gov.uk/cpa-scheme-library)
 * DISA information assurance testing plan requires SIP & H.323 fuzzing: http://www.disa.mil/~/media/Files/DISA/Services/UCCO/rts_ia_test_plan_jan_09.pdf

## Hardening applications

Since fuzzing shows how bad everything is, which mitigations may reduce impact?

* Generate code from specification
 * mitigates if generator and deps are well tested (you don't get more than you asked for)?
 * protobuf is a positive example, ASN.1 as a negative example
* Use "safe" protocols (langsec movement)
* Use "safe" languages (like sane people have been trying to rant since 1950s)
* Compiler hardening flags
* Reduce attack surface
 * Eliminate unncessary functionality
 * (Network/Application) Firewalls in case you take away more than you add
* Upgradeability of components; 3rd party or custom
* KISS
* OpenBSD's malloc.conf
* OpenBSD's [pledge(2)](http://man.openbsd.org/OpenBSD-current/man2/pledge.2) (superior to apparmor/selinux)
* Linux's grsecurity or seccomp
  * Or SELinux (/me runs)
* object permissions (eg protect sensitive data/processes/TCB with filesystem permissions to provide additional layers of defense)
* privsep and sandboxing in general
* engineering/customer incentivisation and education to reject fundamentally unsafe tech
* principle of minimal priviledge
* W^X memory
* NX bit / other processor features?
 * "Control-flow Enforcement Technology"
* https://software.intel.com/sites/default/files/managed/4d/2a/control-flow-enforcement-technology-preview.pdf
* https://blogs.intel.com/evangelists/2016/06/09/intel-release-new-technology-specifications-protect-rop-attacks/
* https://www.endgame.com/blog/mitigating-stagefright-attacks-arm-performance-monitoring-unit

(Almost) all of these are useful as instrumentation for fuzzing.

## How commonly used?

 * couple of "Internet" mega corps do in-house fuzzing
 * sample based most popular free alternative
 * afl popularized nicely (nice entry level drug right now, might develop to more full spectrum antidote)
 * Synopsys Defensics and Peach have largest paying user base
 * ...

## How to popularize?

 * Continuous fuzzing (GitHub / CircleCI, Travis)
 * Shallow fuzzing (vs. deep fuzzing)?
  * AFL gives nice instrant gratification (but currenly may be rather shallow)
 * Make tools easy to use
 * Bug bounty as a service

## How to become an uber-cool fuzz-wizard?

 * Good books/papers etc. to read?
 * Good examples
 * Dockerized tools

## Commercial tools

 * Synopsys: Defensics / SIG
 * Deja Vu Security -> Peach
 * Beyond Security -> BeStorm
 * Spirent -> ex-Mu Dynamics
 * P1 Security -> SS7 focused telecom fuzzing

## Free or open source tools

 * [QuickFuzz](http://quickfuzz.org/)
 * (Chinese) AlphaFuzzer (http://blog.topsec.com.cn/ad_lab/alphafuzzer/)
 * [honggfuzz](https://github.com/google/honggfuzz)
 * [zzuf](http://caca.zoy.org/wiki/zzuf)
 * [mittn](https://github.com/F-Secure/mittn)
 * [covfuzz](https://github.com/attekett/covFuzz)
 * [surku](https://github.com/attekett/Surku)
 * [radamsa](https://github.com/aoh/radamsa)
 * [AFL](http://lcamtuf.coredump.cx/afl/)
 * libfuzzer (http://llvm.org/docs/LibFuzzer.html)
 * QuickCheck (and other language ports)
 * CERT-CC [BFF](https://www.cert.org/vulnerability-analysis/tools/bff.cfm?) & [FOE](https://www.cert.org/vulnerability-analysis/tools/foe.cfm?)
 * [Driller](https://www.internetsociety.org/sites/default/files/blogs-media/driller-augmenting-fuzzing-through-selective-symbolic-execution.pdf) (Looks like not yet open source, but idea looks legit.)
 * jFuzz (NASA Java Pathfinder concolic fuzzer)

## Coverage (white/black-box)

 * What is good coverage for fuzzing anyway? (https://www.inf.ethz.ch/personal/basin/pubs/issta13.pdf claims to have answers)
  * [asancoverage](http://clang.llvm.org/docs/SanitizerCoverage.html)
  * AFL instrumentation (reusable in other projects)
  * AFL's QEMU "user space emulation" mode( https://www.nccgroup.trust/us/about-us/newsroom-and-events/blog/2016/june/project-triforce-run-afl-on-everything/ )
 * Intel PT
  * honggfuzz has pretty good implementation, can use external fuzzing engine like radamsa with it
 * Built-in (compile time) coverage hooks vs. emulator or processor level coverage
 * Pure interpreters are a problem for processor level coverage, JIT helps a bit
 * Custom instrumentation

## Post crash actions

 * duplicate removal by stack trace
 * delta debugging
 * Minimize?
  * [Nipsu](https://github.com/attekett/nipsu)
 * Improve samples?
 * Crash analysis?
  * [!exploitable](https://msecdbg.codeplex.com/) (Windows)
  * [CrashWrangler](https://developer.apple.com/library/mac/technotes/tn2334/_index.html) (OS X)
  * [Exploitable GDB extension](https://github.com/jfoote/exploitable) (Linux) (also [CERT Triage Tools](https://www.cert.org/vulnerability-analysis/tools/triage.cfm?))
 * Automatic exploit generation (/ further auto exploitation?) http://www.scs.stanford.edu/brop/ ?

##  Effectiveness?

 * Dimishes? Still works? **Improves**?
  * Few libraries fuzzed to "death" (except when next improved instrumentation appears)
  * Myriad of other implementations finally touched by fuzzing
 * No issues found (anymore) -> Is fuzzing still useful or is it important quality control?
  * Regressions

## Managed/Script languages

 * Still worth fuzzing
  * Less managed Deps
  * Unexpected failures of 3rd kind
  * Used native code
  * Undocumented features. (Use coverage)
 * apps plenty worth fuzzing (input validation, missing perm checks, info leaks, DoS etc)

## Functional languages

 * Property-based testing / QuickCheck (Haskell) / ScalaCheck (Scala), Rust and Erlang and Clojure also have versions
 * No side effects
 * Mathematically correct functions
 * Any need for fuzzing? (Definitely YES)
 * Fuzzing for unexpected behavior
  * If return value is sum type, is exception error?
 * Business logic failures? (Difficult to instrument)

## Other / misc

 * Fuzzing state machines of stateful protocols
 * Finding side channels?
 * New/emerging targets for fuzzing?
 * Competing/complementary methodologies? Or has the concept of "fuzzing" grown to encompass most of them?
 * Low hanging fruit directs development of fuzzing tech (eg. most effort spent on tooling for unsafe languages)
