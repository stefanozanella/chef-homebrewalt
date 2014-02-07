# Description

This cookbook installs [Homebrew](http://mxcl.github.com/homebrew/) and [Homebrew Cask](https://github.com/phinze/homebrew-cask)
and replaces MacPorts as the *default package provider* for the package resource on OS X systems.

This cookbook is an alternative implementation based upon the Homebrew cookbook maintained by Opscode and the pivotal_workstation cookbook by Pivotal Labs because the default Opscode cookbook did not work as needed for use in Kitchenplan.

# Installation

Add the following to your Cheffile

```
cookbook "homebrewalt",              :github => "kitchenplan/homebrew",            :ref => "v1.7"
```

# Platform

* Mac OS X (10.6+)

The only platform supported by Homebrew itself at the time of this
writing is Mac OS X. It should work fine on Server edition as well,
and on platforms that Homebrew supports in the future.

# Resources and Providers

## package / homebrewalt\_package

This cookbook provides a package provider called `homebrewalt_package`
which will install/remove packages using Homebrew. This becomes the
default provider for `package` if your platform is Mac OS X.

As this extends the built-in package resource/provider in Chef, it has
all the resource attributes and actions available to the package
resource. However, a couple notes:

* Homebrew doesn't have a purge, but the "purge" action will
  act like "remove".

### Examples

    package "mysql" do
      action :install
    end

    homebrewalt_package "mysql"

    package "mysql" do
      provider Chef::Provider::Package::Homebrew
    end

## homebrewalt\_tap

LWRP for `brew tap`, a Homebrew command used to add additional formula
repositories. From the `brew` man page:

    tap [tap]
           Tap a new formula repository from GitHub, or list existing taps.

           tap is of the form user/repo, e.g. brew tap homebrew/dupes.

Default action is `:tap` which enables the repository. Use `:untap` to
disable a tapped repository.

### Examples

    homebrewalt_tap "homebrew/dupes"

    homebrewalt_tap "homebrew/dupes" do
      action :untap
    end

## homebrewalt\_cask

LWRP for `brew cask`, a Homebrew-style CLI workflow for the administration
of Mac applications distributed as binaries. It's implemented as a homebrew
"external command" called cask.

[homebrew-cask on GitHub](https://github.com/phinze/homebrew-cask)

### Examples

    homebrewalt_cask "google-chrome"

    homebrewalt_cask "google-chrome" do
      action :uncask
    end

Default action is `:cask` which installs the Application binary . Use `:uncask` to
uninstall a an Application.

[View the list of available Casks](https://github.com/phinze/homebrew-cask/tree/master/Casks)


# Usage

We strongly recommend that you put "recipe[homebrewalt]" in your node's
run list, to ensure that it is available on the system and that
Homebrew itself gets installed. Putting an explicit dependency in the
metadata will cause the cookbook to be downloaded and the library
loaded, thus resulting in changing the package provider on Mac OS X,
so if you have systems you want to use the default (Mac Ports), they
would be changed to Homebrew.

The default itself ensures that Homebrew is installed and up to date.

# License and Author

* Author:: Graeme Mathieson (<mathie@woss.name>)
* Author:: Joshua Timberman (<joshua@opscode.com>)
* Author:: Pivotal Labs (<accounts@pivotallabs.com>)
* Author:: Roderik van der Veer (<roderik.van.der.veer@kunstmaan.be>)

* Copyright:: 2011, Graeme Mathieson
* Copyright:: 2012, Opscode, Inc <legal@opscode.com>
* Copyright:: 2009, Pivotal Labs <accounts@pivotallabs.com>
* Copyright:: 2013, Roderik van der Veer <roderik.van.der.veer@kunstmaan.be>

## The parts from Opscode are licenced under:

Licensed under the Apache License, Version 2.0 (the "License"); you may
not use this file except in compliance with the License. You may obtain
a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

## The parts from Pivotal Labs and myself are licensed under:

The MIT License

Copyright (c) 2009-2013 Pivotal Labs
Copyright (c) 2013 Roderik van der Veer

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
