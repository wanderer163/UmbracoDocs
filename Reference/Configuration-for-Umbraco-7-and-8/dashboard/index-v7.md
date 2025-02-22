---
versionFrom: 7.0.0
versionRemoved: 8.0.0
---

# Dashboard

As with the other .config files in the `/config` directory the Dashboard.config file lets you customize a portion of the Umbraco experience. In this case the Dashboard.config file controls what shows up in the Dashboard section of the UI when a section of the site loads. The Dashboard is the area on the right side of the UI where most of the data entry and functional interaction takes place, [see examples](../../../Extending/Dashboards/index.md).

By default, Umbraco shows a blank Dashboard when a new section loads. It only shows a form when you take action within the section (i.e. when you click on a node in the Content section, the Dashboard shows the form to update that node's data). But what if you wanted to present your users with some options even before they click on a node?  Well, that is what the Dashboard.config allows you to do.

## Layout

Like the other .config files Dashboard.config is an XML file with a fairly straightforward layout as seen below.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<dashBoard> <!-- Root of the dashboard XML tree -->
    <section>  <!-- Defines a dashboard layout for a group of sections -->
        <areas> <!-- Declares which sections (i.e. content,media,users,[your own]-->
            <area>[area name]</area> <!-- A section to apply this to -->
            ...
        </areas>

        <tab caption="[caption]"> <!-- Creates a tab in the Dashboard with the assigned caption -->
            <control>[path]</control> <!-- What control(v6) / AngularJS View (v7) to load in that tab -->
        </tab>
        ...
    </section>
    ...
</dashBoard>
```

## Section

:::note
This is different from an Umbraco UI Section
:::

Delimits dashboard information to apply to one or more sections. The Dashboard.config may include multiple sections.

## Areas

Defines to which sections of the Umbraco UI to apply the subset of dashboard information.

`<area>` - Always lowercase!

The name of the Umbraco UI Section where you want your user control to be displayed (e.g. content, media, developer, settings, members or a custom section name). You can add your controls to more than one section by adding multiple `<area>` nodes.

The area with the name 'default' is the first dashboard shown when a user logs in, *no matter which sections the user has access to.*

:::warning
Make sure you include the name of your app in lowercase!
:::

## Tab

Defines a page tab that you would like your user control to be added to. The attribute 'caption' defines the text displayed on the tab.  There can be multiple tabs for each Dashboard "page" control/view.

## Control

In Umbraco 6, this setting defines the path to the user control you would like to be displayed on a tab.
In Umbraco 7 this is the path to an AngularJS view.

## Access / Permissions

The `<access />` element makes it possible to set permissions on sections, tabs and controls and you can either grant or deny certain user types access.

It works by adding an `<access />` node under either a `<section />`, `<tab />` or `<control />` node.

As children of `<access />` you can either add `<grant />` which grants permissions to those types of users (AND automatically denies access to those who are not there!).

`<grantBySection />` which grants permissions to those users who has access to specific sections. This can be useful for more granular permissions.

`<deny />` which denies permissions to those types of users (AND automatically grants everyone else).

No matter the settings, the root user (id:0) can see everything, so don't panic if you set deny permissions for administrators and they are still are able to see everything ;-)

Example on permissions:

```xml
<tab caption="Last Edits">
    <access>
        <grant>writer</grant>
        <grant>editor</grant>
        <grantBySection>content</grantBySection>
    </access>
    <control>/usercontrols/dashboard/latestEdits.ascx</control>
</tab>
```

## Customizing

In order to customize the dashboard in Umbraco, one needs to do a couple of things.

:::note
Older version can use a .NET `UserControl`. See [creating dashboards with usercontrols](index-v6.md)
:::

## Create an AngularJS View

The Dashboard will load one or more AngularJS views and display them as a series of tabs. It is recommended that you store your views in a subfolder within the App_Plugins folder.

## Update the Dashboard.config

Once you have created the AngularJS View that you want to have loaded you must update the `Dashboard.config`. This is to tell Umbraco to load your View when a user enters a new section. If you are doing this for yourself all you need to do is edit the `Dashboard.config` on your site to add the views. Are you adding a section to go with a package, you will want to include a Package Action to update the `Dashboard.config` during install.

See the [Packaging reference Actions](../../../Extending/Packages/package-actions.md) for more information.

## Sample

Below is an example of a valid Dashboard.config:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<dashBoard>
    <section>
        <areas>
            <area>content</area>
        </areas>
        <tab caption="Welcome">
            <control>/app_plugins/mycustomdashboard/customwelcome.html</control>
        </tab>
    </section>
</dashBoard>
```

[Check out our Creating a Custom Dashboard tutorial](../../../Tutorials/Creating-a-Custom-Dashboard/index.md)
