---
title: "Managing the search box in SharePoint sites"
ms.author: keremy
author: jeffkizn
manager: parulm
ms.audience: Admin
ms.topic: article
ms.service: mssearch
ms.localizationpriority: medium
search.appverid:
- BFB160
- MET150
- MOE150
description: "How to customize the search box experience on SharePoint sites"
---

# Search box settings on SharePoint sites

One of the several ways Microsoft Search can be customized on SharePoint sites is to tailor how the search box in the suite navigation bar works in SharePoint sites to best fit your needs.

For other customization options, see [Changing the Microsoft Search results page to add custom verticals, result types and layouts](customize-search-page.md), and [Creating a custom search results page](create-search-results-pages.md).

> [!NOTE]
> The suite navigation bar search box is not available for all customers at this time, but these options can still be set now and they will take effect when it becomes available.

For the tasks listed below, you will use PowerShell with SharePoint PnP PowerShell extensions. You can install and learn more about how to get started [here](/powershell/sharepoint/sharepoint-pnp/sharepoint-pnp-cmdlets?view=sharepoint-ps). You will sign into your site or site collection using this command:

```powershell
Connect-PnPOnline -Url <yoursiteurl> -UseWebLogin
# this will prompt you to sign into your site. Use the site owner credentials 
```

## Changing the scope of search

When you create a new site in SharePoint Online today, and type into the search box, you are taken to the Microsoft Search results page. This page shows results from your current site by default and allows you to expand the scope of your search to the hub that the current site is associated with (if there is one), or to the whole organization.

The scope the search box uses, by default, depends on type of site.

* Regular sites search over the current site.
* Hub sites search over all sites in the hub.
* Home sites search over all content.

In some cases, you may want to change these defaults to always search over the whole organization, or across the hub a site is associated with, without needing an additional click.

As a site owner, you can change these defaults using the following command:

```powershell
Set-PnPSearchSettings -SearchScope Tenant
# DefaultScope | Hub | Site | Tenant
```

After running this command, the site that was previously showing results from the current site by default will start to show results from the whole organization.

To go back to the default setting, run the command again with the value “DefaultScope". To search across the Hub, use “Hub” as the SearchScope value.

This setting applies at the individual site level. There is no equivalent setting for site collections.

## Show or hide the search box

You can choose to hide the suite navigation bar search box if you want to prevent your users from searching or to use a custom search box implementation.

To change this setting for a given site use this command:

```powershell
Set-PnPSearchSettings -Scope Web -SearchBoxInNavBar Hidden
# Hidden | Inherit
```

Alternately, if you want to set it for all the sites in a site collection, you can use this command:

```powershell
Set-PnPSearchSettings -Scope Site -SearchBoxInNavBar Hidden
# Hidden | Inherit
```

After running these commands, the search box will no longer show up in the navigation bar on top of your page. To go back to showing the search box, run the commands again with the value provided to "SearchBoxInNavBar" parameter to “Inherit”.

There are several points to consider:

* This setting only applies to the search box in the suite navigation bar. It does not apply to search boxes that are in the page, or to search boxes on classic pages.

* Once you’ve disabled the search box in the navigation bar, if you want search functionality in your site, you will have to provide it yourself using a custom web part or a SharePoint Framework extension.

* This solution will remove the search box from lists and libraries for your site as well. Your custom search solution will need to consider contextual searches for SharePoint lists and libraries, in addition to site-wide search.

* If you apply the setting to the root site of your domain, the SharePoint start page will also stop showing the search box.

## Changing the hint displayed in the search box

You can change the hint the search box shows for a given site or site collection. This is the text that appears in the search box before they start typing into it. This may help guide your users about what to expect from search if you’ve configured a custom results page or changed behavior of search in other ways.

> [!NOTE]
> To be able to make this change, you need to allow running custom scripts on the site in question as a tenant administrator, which is disallowed by default. Please see https://docs.microsoft.com/sharepoint/allow-or-prevent-custom-script for details. You can allow running custom scripts, make the change, and then revert to disallowing scripts for the site if necessary.

To change this setting for a given site run the following command:

```powershell
Set-PnPSearchSettings -Scope Web -SearchBoxPlaceholderText "my placeholder" 
```

Alternately, if you want to set it for all the sites in a site collection, you can use this command:

```powershell
Set-PnPSearchSettings -Scope Site -SearchBoxPlaceholderText "my placeholder" 
```

To go back to the default placeholder text, set the value to be blank ("").