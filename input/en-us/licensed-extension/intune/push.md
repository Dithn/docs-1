---
Order: 40
xref: intune-push
Title: Pushing Converted Intune Packages
---

<?! Include "../../../shared/intune-note.txt" /?>

## Summary

The ability to push Chocolatey Intune packages (packages with the extension `.intunewin`) has been added to the `push` command. The command will push the specified Chocolatey Intune package, any dependencies specified for the package along with the core Chocolatey Intune packages of  `chocolatey`, `chocolatey-license` and `chocolatey.extension` when the packages do not already exist on the Intune tenant. The `chocolatey-license` package will be automatically generated using the Chocolatey for Business license that is present on the local computer running the `push` command, if it does not exist in the Intune tenant.

## Usage

### Command Line

~~~sh
choco push <path to intune pkg> [<options/switches>]
~~~

When using the `push` command the package path provided must be an `.intunewin` file that was previously converted to a Chocolatey Intune package using the [`convert`](xref:intune-convert) command. This will push the Chocolatey Intune package, and its dependencies if they do not already exist there, to the Intune tenant.

### Context Menu

When you right-click on a Chocolatey Intune package (which is a package with the extension of `.intunewin`), select **Upload/Push Intune Package...** to push the package to the Intune tenant. Alternatively, you can double-click the Chocolatey Intune package to upload it, and all of its dependencies, to the Intune tenant.

### Dependency Resolving

When resolving dependencies for the Chocolatey Intune package, Chocolatey will look for the dependency package(s), with the same ID and within the version range, in order in the locations below:

1. The Intune tenant.
2. The path of the package you want to push.

If no packages are found in these locations, the `push` command will fail, and the error will state the dependency package that cannot be found.

### Examples

~~~sh
choco push firefox.86.0.intunewin --source=<INTUNE TENANT GUID> --api-key=<TENANT CLIENT ID>:<TENANT CLIENT SECRET>
~~~

To use a `source` and `api-key` without specifying them each time, set them in the Chocolatey config using:

~~~sh
choco config set --name=intuneTenantGUID --value=<INTUNE TENANT GUID>
choco apikey --source=<INTUNE TENANT GUID> --key=<TENANT CLIENT ID>:<TENTANT CLIENT SECRET>
~~~

Once that is done you can push the package as normal:

~~~sh
choco push firefox.86.0.intunewin
~~~

> :warning: **WARNING**
>
> By specifying the path to a pre-release package, pre-release support will be enabled for the package and its dependencies. However, while the package you are pushing can be a pre-release, you cannot have a dependency on a pre-release package if the Chocolatey package you are pushing is a stable release. This is the same behaviour as the [`convert`](xref:intune-convert) command.

> :warning: **WARNING**
>
> If this is the first time pushing a package to your Intune tenant, make sure that you also have the converted packages of `chocolatey` and `chocolatey.extension` available in the same directory as the package you want to push.

> :memo: **NOTE**
>
> If a package for `chocolatey-license` does not already exist on the specified Intune tenant, a new package will be created using the Chocolatey for Business license that is present on the local computer the command is run from.

## FAQ

Frequently asked questions about choco push for Intune can be found [here](xref:intune-faq#push-faqs)
