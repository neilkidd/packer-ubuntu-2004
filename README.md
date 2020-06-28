# packer-ubuntu-2004

[![Build Status](https://travis-ci.com/neilkidd/packer-ubuntu-2004.svg?branch=master)](https://travis-ci.com/neilkidd/packer-ubuntu-2004) [![MIT License](https://img.shields.io/github/license/twbs/bootlint.svg)](https://github.com/twbs/bootlint/blob/master/LICENSE) 

This repository contains [Packer](https://packer.io/) templates for creating, and publishing, Ubuntu 20.04 [Vagrant](https://www.vagrantup.com/) boxes based on [Virtualbox](https://www.virtualbox.org/).

## Goals

There are a good number of public vagrant boxes, however each one has some issue that made them unsuitable for my *out of box* usage. I wanted to experiment freely, and iterate rapidly, so decided to build and [publish](https://app.vagrantup.com/neil_kidd) my own with the following goals:

- [x] English GB localisation. (Locale and keyboard)
- [x] Ubuntu LTS OOB experience to mimic a locally installed server.
- [x] All Updates applied.
- [ ] Ubuntu-mate-core desktop
- [ ] Try Github actions to auto build and publish up to date boxes.

### Credits

- Jeff Geerling's excellent [geerlingguy/packer-boxes](https://github.com/geerlingguy/packer-boxes) repository.
- [boxcutter/ubuntu](https://github.com/boxcutter/ubuntu) repository.

## Published Boxes

- [neil_kidd/ubuntu2004](https://app.vagrantup.com/neil_kidd/boxes/ubuntu2004)

## Requirements

The versions of software I used are in parenthesis. YMMV.

- [Packer](https://www.packer.io/) (1.6.x)
- [Virtualbox](https://www.virtualbox.org/) (6.1.x)
- Optional: [Hashicorp Vagrant Cloud Account](https://app.vagrantup.com/boxes/search), should you want to publish your builds.
- Optional: [Vagrant](https://www.vagrantup.com/) (2.2.x) for local development and usage.

## Local development

To experiment locally, without uploading to Vagrant cloud, provide the packer argument `-except=`.

1. Run:  
`packer build -except=vagrant-cloud ubuntu.json`.  
After a few minutes a ready to use box will be in the builds directory.
1. Edit the provided [Vagrantfile](Vagrantfile) to point to the local box.
1. Run:  
`vagrant up`

## Publishing a Box to Vagrant Cloud

1. Log in to your [Vagrant Cloud Account](https://app.vagrantup.com/boxes/search)
1. Create an [Authentication Token](https://app.vagrantup.com/settings/security)
    1. Save the long string somewhere safe. Later, this will be exported as the env variable `VAGRANT_CLOUD_TOKEN`.
1. Create a new [Vagrant Box](https://app.vagrantup.com/)
    1. __Tip:__ Enter the *Name* as `ubuntu2004`.  
    In the next step, the value for ` "vm_name": ` will not need editing.
1. Edit [ubuntu.json](ubuntu.json), changing the values for:  
`"vagrantcloud_username": "neil_kidd"`  
`"vm_name": "ubuntu2004"`.
1. Paste your Auth Token into the following command and run:  
`VAGRANT_CLOUD_TOKEN={REPLACE_THIS} packer build ubuntu.json`
1. Sometime later the terminal should have output similar to:

    ```bash
    ...
    virtualbox-iso (vagrant-cloud): Box successfully uploaded
    ==> virtualbox-iso (vagrant-cloud): Releasing version: 2004.20200628.082705
    virtualbox-iso (vagrant-cloud): Version successfully released and   available
    Build 'virtualbox-iso' finished.
    ```

1. Edit the provided [Vagrantfile](Vagrantfile) to point to your cloud release.
1. To import the box and experiment, run:  
`vagrant up`

## Notes

### Version Numbers

Version numbers are generated incrementally using a combination of ubuntu release number + UTC date-time.  
e.g: `2004.20200618.203713`  
Where:

- `2004` = the Ubuntu release 20.04
- `20200618` = the 18th of June 2020
- `203713` = 8 PM, 37 minutes, 13 seconds

### Flexible Configuration

It is possible to re-use the templates without editing the provided templates directly. Many of the variables, in [ubuntu.json](ubuntu.json), can be overridden on the command line or using a [secondary json file](https://www.packer.io/docs/templates/user-variables#from-a-file).

#### Example Publishing to Specified Cloud Account

This example will build *as is* and publish to your pre configured Vagrant Cloud account ( `you/myvm` ):

```bash
VAGRANT_CLOUD_TOKEN={your_token} \
packer build \
-var 'vagrantcloud_username=you' \
-var 'vm_name=myvm' \
ubuntu.json
```

#### Example Building a Local Box for the US

This example will build a local box, in US configuration with a 50GB hdd, 2 cpus and 4gb of Ram.

```bash
packer build \
-except=vagrant-cloud \
-var 'keyboard_country=US' \
-var 'locale=en_US.UTF-8' \
-var 'cpus=2' \
-var 'disk_size=50000' \
-var 'memory=4096' \
ubuntu.json
```

## License

[MIT](LICENSE)
