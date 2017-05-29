# Dashboard.config

A dashboard is a tab that appears on the root of an application (section).  If you want to create a dashboard, simply modify this file and point it to your shiny new AngularJs dashboard.

Below is a tiny excerpt from `dashboard.config`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<dashBoard>
  <section alias="StartupSettingsDashboardSection">
    <areas>
      <area>settings</area>
    </areas>
    <tab caption="Welcome">
      <control showOnce="true" addPanel="true" panelCaption="">
        views/dashboard/settings/settingsdashboardintro.html
      </control>
    </tab>
  </section>
  ...
</dashBoard>
```

[<Back 04 - Trees.config](04 - Trees.config.md)

[Next> 06 - FileSystemProviders.config](06 - FileSystemProviders.config.md)
