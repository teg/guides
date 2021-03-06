# Managing repositories

`osbuild-composer` does not inherit the system repositories located in `/etc/yum.repos.d/`. Instead, it has its own set of official repositories defined in `/usr/share/osbuild-composer/repositories`. To use custom repositories, define overrides in `/etc/osbuild-composer/repositories`. This directory is meant for user defined repositories and the files located here take precedence over those in `/usr`. 

The configuration files are not in the usual "repo" format. Instead, they are simple `JSON` files.

*Important note: osbuild-composer can only create images for the distribution and architecture it is running on. For example, if you are running Fedora 33 on x86_64, osbuild-composer will create all images as Fedora 33 for x86_64. Building other distributions and architectures except for the host is not supported.*

## Defining custom repositories 

To set your own repositories, create this directory if it does not exist already:

```
$ sudo mkdir -p /etc/osbuild-composer/repositories
```

Based on the system you are running (see `/etc/os-release` if you are not sure), determine the name of a new JSON file:

* Fedora 32 - `fedora-32.json`
* Fedora 33 - `fedora-33.json`
* Already released RHEL 8 - `rhel-8.json`
* Pre-release RHEL 8.4 - `rhel-84.json`

Then, create the JSON file with the following structure:

```json
{
    "<ARCH>": [
        {
            "name": "<REPO NAME>",
            "metalink": "",
            "baseurl": "",
            "mirrorlist": "",
            "gpgkey": "",
            "check_gpg": "",
            "metadata_expire": "",
        }
    ]
}
```
Specify only one value for the following attributes: `metalink`, `mirrorlist`, or `baseurl`. The remaining fields are optional.

For example, assuming that the host OS is Fedora 33 running on x86_64, create `/etc/osbuild-composer/repositories/fedora-33.json` with this content:
```json
{
    "x86_64": [
        {
            "name": "fedora",
            "metalink": "https://mirrors.fedoraproject.org/metalink?repo=fedora-33&arch=x86_64",
            "gpgkey": "-----BEGIN PGP PUBLIC KEY BLOCK-----\n\nmQINBF4wBvsBEADQmcGbVUbDRUoXADReRmOOEMeydHghtKC9uRs9YNpGYZIB+bie\nbGYZmflQayfh/wEpO2W/IZfGpHPL42V7SbyvqMjwNls/fnXsCtf4LRofNK8Qd9fN\nkYargc9R7BEz/mwXKMiRQVx+DzkmqGWy2gq4iD0/mCyf5FdJCE40fOWoIGJXaOI1\nTz1vWqKwLS5T0dfmi9U4Tp/XsKOZGvN8oi5h0KmqFk7LEZr1MXarhi2Va86sgxsF\nQcZEKfu5tgD0r00vXzikoSjn3qA5JW5FW07F1pGP4bF5f9J3CZbQyOjTSWMmmfTm\n2d2BURWzaDiJN9twY2yjzkoOMuPdXXvovg7KxLcQerKT+FbKbq8DySJX2rnOA77k\nUG4c9BGf/L1uBkAT8dpHLk6Uf5BfmypxUkydSWT1xfTDnw1MqxO0MsLlAHOR3J7c\noW9kLcOLuCQn1hBEwfZv7VSWBkGXSmKfp0LLIxAFgRtv+Dh+rcMMRdJgKr1V3FU+\nrZ1+ZAfYiBpQJFPjv70vx+rGEgS801D3PJxBZUEy4Ic4ZYaKNhK9x9PRQuWcIBuW\n6eTe/6lKWZeyxCumLLdiS75mF2oTcBaWeoc3QxrPRV15eDKeYJMbhnUai/7lSrhs\nEWCkKR1RivgF4slYmtNE5ZPGZ/d61zjwn2xi4xNJVs8q9WRPMpHp0vCyMwARAQAB\ntDFGZWRvcmEgKDMzKSA8ZmVkb3JhLTMzLXByaW1hcnlAZmVkb3JhcHJvamVjdC5v\ncmc+iQI4BBMBAgAiBQJeMAb7AhsPBgsJCAcDAgYVCAIJCgsEFgIDAQIeAQIXgAAK\nCRBJ/XdJlXD/MZm2D/9kriL43vd3+0DNMeA82n2v9mSR2PQqKny39xNlYPyy/1yZ\nP/KXoa4NYSCA971LSd7lv4n/h5bEKgGHxZfttfOzOnWMVSSTfjRyM/df/NNzTUEV\n7ORA5GW18g8PEtS7uRxVBf3cLvWu5q+8jmqES5HqTAdGVcuIFQeBXFN8Gy1Jinuz\nAH8rJSdkUeZ0cehWbERq80BWM9dhad5dW+/+Gv0foFBvP15viwhWqajr8V0B8es+\n2/tHI0k86FAujV5i0rrXl5UOoLilO57QQNDZH/qW9GsHwVI+2yecLstpUNLq+EZC\nGqTZCYoxYRpl0gAMbDLztSL/8Bc0tJrCRG3tavJotFYlgUK60XnXlQzRkh9rgsfT\nEXbQifWdQMMogzjCJr0hzJ+V1d0iozdUxB2ZEgTjukOvatkB77DY1FPZRkSFIQs+\nfdcjazDIBLIxwJu5QwvTNW8lOLnJ46g4sf1WJoUdNTbR0BaC7HHj1inVWi0p7IuN\n66EPGzJOSjLK+vW+J0ncPDEgLCV74RF/0nR5fVTdrmiopPrzFuguHf9S9gYI3Zun\nYl8FJUu4kRO6JPPTicUXWX+8XZmE94aK14RCJL23nOSi8T1eW8JLW43dCBRO8QUE\nAso1t2pypm/1zZexJdOV8yGME3g5l2W6PLgpz58DBECgqc/kda+VWgEAp7rO2A==\n=EPL3\n-----END PGP PUBLIC KEY BLOCK-----\n",
            "check_gpg": true
        }
    ]
}
```
