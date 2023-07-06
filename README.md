# Splunk Universal Forwarder - The Missing Pkg Template
Splunk's universal forwarder is generally a useful tool, but requires some fiddling around to package correctly. I figured I would share my template (PRs welcome!) that is _slightly_ opinionated. 

## Assumptions Made 
- You want the installation to be at /var/splunkforwarder
- You want the forwarder to run as root via LaunchDaemon
- You don't want the GUI launcher to be installed along side
- You have [munkipkg](https://github.com/munki/munki-pkg) installed

## How to use this 
0. Clone the repo and sync the included Bom. This will ensure the directory structure and modes are set properly (these can get lost in git).

```munkipkg --sync .```

1. Download the version of the Splunk Universal Forwarder you want to deploy from Splunk (note: this repo does NOT contain a copy of this because it's not mine to distribute). You want the tar.gz formatted donwload, not the DMG
2. Expand the contents of the tar.gz archive into the payload/var/splunkforwarder directory
3. Remove the quarantine bit from the binaries, else you will end up prompting users with Gatekeeper by accident.

```xattr -cr payload/var/splunkforwarder```

4. Edit build-info.plist to reflect the correct version of the UF you are deploying
5. Build the pkg with munkipkg and import to Munki or your distribution tool of choice.
