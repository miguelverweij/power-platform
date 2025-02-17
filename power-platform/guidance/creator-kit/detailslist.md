---
title: DetailsList control reference | Creator Kit
description: Learn about the details and properties of the DetailsList control in the Creator Kit.
author: denisem-msft
manager: devkeydet
ms.component: pa-maker
ms.topic: conceptual
ms.date: 05/16/2022
ms.subservice: guidance
ms.author: demora
ms.reviewer: tapanm
search.audienceType: 
  - maker
search.app: 
  - D365CE
  - PowerApps
  - Powerplatform
contributors:
  - tapanm-msft
  - slaouist
---

# :::no-loc text="DetailsList"::: control

[This article is pre-release documentation and is subject to change.]

A control used to display a set of data.

:::image type="content" source="media/details-list.png" alt-text="DetailsList control.":::

## Description

A details list (`DetailsList`) is a robust way to display an information-rich collection of items and allow people to sort, group, and filter the content. Use a `DetailsList` when information density is critical.

The `DetailsList` code component allows using the [Fluent UI `DetailsList` component](https://developer.microsoft.com/fluentui#/controls/web/detailslist) from inside canvas apps and custom pages.

> [!NOTE]
> Component source code and more information in the [GitHub code components repository](https://github.com/microsoft/powercat-code-components/tree/main/DetailsList).

## Limitations

This code component can only be used in canvas apps and custom pages.

## Key properties

| Property | Description |
| -------- | ----------- |
| `Items` | Required. The data source items table to render. Items can be from any data source because values are mapped in the Columns property (which acts as a schema definition). |
| `Fields` | Required. The fields that are needed. |
| `Columns` | Required. Table mapping definition between the component column and the data source. Use this property to map field names and define specific column behavior. |

## Additional properties

| Property | Description |
| -------- | ----------- |
| `Views` | View if supported by the data source (for example, Dataverse). |
| `Selection type` | Controls how and whether the `DetailsList` manages selection. Options include none, single, multiple |
| `Select rows on focus` | Determines whether rows will be selected when the control is focused. |
| `Page size` | The number of items displayed in the page. |
| `Sort column` | ColName value to sort, by default. |
| `Sort direction` | Default sorting direction. |
| `Compact` | Determines whether to render in compact mode. |
| `Header visible` | Controls the visibility of the header. |
| `Alternate row color` | The color of every other row. Accepts CSS color values (for example,  hexadecimal, RGB, predefined) |
| `Selection radio button` | Determines whether to render the **Select all** radio button. |
| `Raise OnRowSelection` | Enable this property to allow **OnRowSelection** events. |

## Mapping data to columns

To determine which columns are displayed in the `DetailsList`, configure the following properties of the `DetailsList`:

1. **Fields**. Add the fields you want by selecting the Edit option in the controls flyout menu on the right (this uses the same interface for modifying [predefined data cards](/power-apps/maker/canvas-apps/working-with-cards)).

1. **Columns**. Provide specific mapping between columns and fields in the `Columns` property.  

The following table schema must be used in the `Columns` (`column_Items`) property to display the data you want.

### Columns table schema

| Name | Description |
| ------ | ----------- |
| `ColName` | A unique key for identifying the column. |
| `ColDisplayName` | Name to render on the column header. |
| `ColWidth` | Minimum width for the column. |
| `ColSortable` | Determines whether the column has sorting behavior. |
| `ColIsBold` | Determines whether the text is bold or not. |
| `ColResizable` | Determines whether the column can be resized. |
| `ColShowAsSubTextOf` | The ColName value of the parent column this text. Leave blank to display in a separate column. |
| `ColCellType` | Provide **link** as the value to make the column selectable. Leave blank for regular text without style. |

Example:

Mapping to the Dataverse [Accounts](/power-apps/developer/data-platform/reference/entities/account) system table, with the following formula:

```powerapps-dot
Table(
    {
        ColName: "name",
        ColDisplayName: "Name",
        ColWidth: 200,
        ColSortable: true,
        ColIsBold: true,
        ColResizable: true
    },{
        ColName: "address1_city",
        ColDisplayName: "City:",
        ColShowAsSubTextOf: "name"
    },{
        ColName: "address1_country",
        ColDisplayName: "Country:",
        ColShowAsSubTextOf: "name"
    },{
        ColName: "telephone1",
        ColDisplayName: "Telephone",
        ColWidth: 100,
        ColSortable: true,
        ColResizable: true
    },{
        ColName: "primarycontactid",
        ColDisplayName: "Primary Contact",
        ColWidth: 200,
        ColSortable: true,
        ColSortBy: "_primarycontactid_value",
        ColResizable: true,
        ColCellType: "link"
    }
)
```

## Configure "On Change" behavior

Add and modify the following formula in the component's `OnChange` property to configure specific actions based on the `EventName` provided by the component:

- Trigger events when a user changes the selected row: Enable the property **Raise OnRowSelectionChange event** in the component.
- Configure link behavior: Add columns with the **ColCellType** value set to **link**.

```powerapps-dot
/* Runs when selected row changes and control property 'Raise OnRowSelection event' is true */
If( Self.EventName = "OnRowSelectionChange",
    Notify( "Row Select " & Self.EventRowKey )
);

/* Runs when a user selects a column with ColCellType set to 'link' */
If( Self.EventName = "CellAction",
    Notify( "Open Link " &  Self.EventColumn & " " & Self.EventRowKey )
)
```

## Best practices

Go to [Fluent UI DetailsList control best practices](https://developer.microsoft.com/fluentui#/controls/web/detailslist).

[!INCLUDE[footer-include](../../includes/footer-banner.md)]