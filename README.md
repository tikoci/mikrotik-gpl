# mikrotik-gpl

> This project is a work in progress, with goal to help ensure GPL complaince for MikroTik RouterOS.  

MikroTik does not currently publish GPL source code, specifically the Linux kernel, on its website or other publicly downloadable place.  Instead, it requires a email request to obtain the source code.  So this repo was created to automate the process and provide it for direct download from GitHub when source is provided by MikroTik.

> [!WARNING]
> At time of writing, the only GPL sources received are dated from 2022, and as response to a request for "RouterOS 7.18.2" GPL source.  Upon inquiry, MikroTik support suggested on March 20, 2025, "[w]e are working on a newer patch".

TIKOCI will continue to advocate for MikroTik providing regular, on-going, per-build disclosure of GPL/other sources, as required under various open source licenses.

> [!TIP]
> This project is focused on current and new versions of RouterOS, although older versions contained GPL code.  Use other resources for older MikroTik GPL disclosures.  For example RouterOS 6.41rc38 was found on this unrelated GitHub project, from 8 years ago:
> https://github.com/robimarko/routeros-GPL


### Implementation Details

When the GPL/open source code is obtained from MikroTik, it will be manually checked into this repo.    So this project mainly _stores_ MikroTik's GPL disclosure publicly.  So the only "code" here is a GitHub Action, `gpl-nag-email.yaml`.  The build script checks MikroTik website for the current "stable" version of RouterOS.  If a version is NOT found in GitHub [Releases](https://github.com/tikoci/mikrotik-gpl/releases), the build will email MikroTik to obtain the GPL source.  The build is scheduled to check for new stable releases twice weekly, and will not send emails if a version has already been requested.

When MikroTik response with source, the provided disclosure will be in Releases.  Additional, the expanded contents will be stored in repo by RouterOS version, or date received if version is unclear.  

### Potential Uses

> [!NOTE]
> Significant parts of RouterOS are NOT GPL or other open source code, and would not require disclosure. 
> RouterOS does use the Linux kernel, so that is main GPL-based code, and will be posted here when obtained from MikroTik.  But the kernel alone is not enough to "rebuild" RouterOS or "add custom packages", as most of Linux "userland" is proprietary code and/or may use non-public 3rd party drivers.  So the code here may not be useful, depending on your goals.

GPL and other open source licenses do not require a reason to want disclosure.  

*  a `diff` could be used similar to [tikoci/restraml's "Scheme Tools"](https://tikoci.github.io/restraml) that use  `/console/inspect`, and similarly enable better visibility into any kernel changes – providing another view into the opaque releases notes
* Run GitHub basis static code analysis tools against disclosed GPL code - even if not great, another datapoint on a new release 
* Perhaps useful in various "re-packaging" CHR/X86 project in TIKOCI - perhaps improving EUFI and knowing kernel options used to better align virtualization settings
