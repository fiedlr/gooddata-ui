---
id: ranking_filter_component
title: Ranking Filter
sidebar_label: Ranking Filter
copyright: (C) 2007-2020 GoodData Corporation
---

The **Ranking Filter component** is a dropdown component that allows you to create a new [ranking filter](30_tips__filter_visual_components.md#ranking-filter) or to edit an existing one. When a user clicks **Apply**, a callback function that contains a ranking filter ready to be used in the AFM is called.

![Ranking Filter Component](assets/ranking_filter_combined.png "Ranking Filter Component")

## Structure

```jsx
import "@gooddata/sdk-ui-filters/styles/css/main.css";
import { MeasureValueFilter } from "@gooddata/sdk-ui-filters";

<RankingFilter
  onApply={<on-apply-callback>}
  onCancel={<on-cancel-callback>}
  filter={<filter>}
  buttonTitle={<toggle-button-title>}
  measureItems={<visualization-measures>}
  attributeItems={<visualization-attributes}
/>
```     

## Example

The following example shows a table displaying one measure sliced by one attribute. A user can use the Ranking Filter component to filter the displayed bars and see the relevant data only.

```jsx
import React, { Component } from "react";
import "@gooddata/sdk-ui-filters/styles/css/main.css";
import "@gooddata/sdk-ui-charts/styles/css/main.css";
import { RankingFilter } from "@gooddata/sdk-ui-filters";
import { Ldm } from "./ldm";

const measureTitle = "$ Total Sales";

export default class SalesByResort extends Component {
    this.state = { filters: [ newRankingFilter(Ldm.$TotalSales, "TOP", 3) ] };

    onApply = filter => {
        this.setState({ filters: [filter] });
    };

    render() {
        const { filters } = this.state;

        return (
            <div>
                <RankingFilter
                    onApply={this.onApply}
                    filter={filters[0]}
                    buttonTitle={measureTitle}
                />
                <PivotTable
                    measures={[Ldm.$TotalSales]}
                    rows={[Ldm.LocationResort]}
                    filters={filters}
                />
            </div>
        );
    }
}
```
## Properties

| Name                         | Required? | Type                                                            | Default                                   | Description                                                                                                                                                                                                                                                                                                                                   |
| :--------------------------- | :-------- | :-------------------------------------------------------------- | :---------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| measureItems                 | true      | Object with measure's `title` (string) and `localIdentifier` (string) properties.                                                                |                                           | The list of available measures from which user can choose the measure on which the filter is applied. Typically a list of measures used in the visualization filter is supposed to filter.                                                                                                    |
| attributeItems                 | false      | Object with attribute's `title` (string), `localIdentifier` (string) properties. Optional property `type` (with possible string value `"ATTRIBUTE"` or `"DATE"`) affects rendered option's icon.                                                               |                                           | The list of available attributes from which user can choose the attribute on which the filter is applied. Typically a list of attribute used in the visualization that defines visualization granularity. When property is not provided, the filter behaves as if all the attributes were selected (visualization default granularity was set.|
| filter                       | true      | [Filter](30_tips__filter_visual_components.md#ranking-filter) |                                           | The ranking filter definition                                                                                                                                                                                                                                                                                                           |
| onApply                      | true      | Function                                                        |                                           | A callback when the selection is confirmed by a user. The passed configuration of the ranking filter is already transformed into a ranking filter definition, which you can then send directly to a chart.                                                                                                                        |
| onCancel                     | false     | Function                                                        |                                           | A callback when a user clicks the Cancel button or makes the dropdown close by clicking outside of it                                                                                                                                                                                                                                        |
| buttonTitle                  | false     | string                                                          |                                           | The title of the toggle button                                                                                                                                                                                                                                                                                                                           |
| locale                       | false     | string                                                          | `en-US`                                   | The localization of the component. See the [full list of available localizations](https://github.com/gooddata/gooddata-ui-sdk/blob/master/libs/sdk-ui/src/base/localization/Locale.ts).                                                                                                                                                                        |

## Custom toggle button

If you want to use your own custom button for toggling the filter dropdown, use the Ranking Filter Dropdown component. This component renders only the dropdown body outside of the current DOM tree using [portals](https://reactjs.org/docs/portals.html).

![Custom button](assets/ranking_filter_custom_button.png "Custom button")

The component has all the same properties as the Ranking Filter component (see [Properties](#Properties)) with the following exceptions:
* The `buttonTitle` property is irrelevant for the Ranking Filter Dropdown component.
* The `onCancel` property is mandatory for the Ranking Filter Dropdown component, because it is supposed to be used to hide the dropdown.
* The Ranking Filter Dropdown component has one additional property, `anchorEl`. This optional property specifies the element that the dropdown is aligned to, which is typically your toggle button. The property can be an event target or a string and defaults to `'body'`.

Check out our [live examples](https://github.com/gooddata/gooddata-ui-sdk/tree/master/examples/sdk-examples) for demonstration.