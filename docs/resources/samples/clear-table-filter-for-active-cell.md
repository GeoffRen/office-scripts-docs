---
title: 'Clear table column filter based on active cell location'
description: 'Learn how to clear table column filter based on active cell location.'
ms.date: 03/04/2021
localization_priority: Normal
---

# Clear table column filter based on active cell location

This sample clears the table column filter based on the active cell location. The script detects if the cell is part of a table, determines the table column, and clears any filter that are applied on it.

If you wish to learn more about how to save the filter prior to clearing it (and re-apply later), see [Move rows across tables by saving filters](move-rows-across-tables.md), a more advanced sample.

_Before clearing column filter (notice the active cell)_

![Before clearing column filter](../../images/before-filter-applied.png)

_After clearing column filter_

![After clearing column filter](../../images/after-filter-cleared.png)

## Sample code: Clear table column filter based on active cell

The following script clears the table column filter based on active cell location and can be applied to any Excel file with a table. For convenience, you can download and use <a href="table-with-filter.xlsx">table-with-filter.xlsx</a>.

```TypeScript
function main(workbook: ExcelScript.Workbook) {
    // Get active cell.
    const cell = workbook.getActiveCell();

    // Get all tables associated with that cell.
    const tables = cell.getTables();
    
    // If there is no table on the selection, return/exit.
    if (tables.length !== 1) {
      console.log("The selection is not in a table.");
      return;
    }

    // Get table (since it is already determined that there is only
    // a single table part of the selection).
    const currentTable = tables[0];

    console.log(currentTable.getName());
    console.log(currentTable.getRange().getAddress());

    const entireCol = cell.getEntireColumn();
    const intersect = entireCol.getIntersection(currentTable.getRange());
    console.log(intersect.getAddress());

    const headerCellValue = intersect.getCell(0,0).getValue() as string;
    console.log(headerCellValue);

    // Get column.
    const col = currentTable.getColumnByName(headerCellValue);

    // Clear filter.
    col.getFilter().clear();
}
```

## Training video: Clear table column filter based on active cell location

For an example of how to work with ranges, see [Range basics training videos](range-basics.md#training-videos-range-basics).
