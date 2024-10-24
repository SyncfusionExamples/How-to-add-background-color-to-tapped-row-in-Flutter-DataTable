# How to add background color to tapped row in Flutter DataTable (SfDataGrid)?.

In this article, we will show you how to add background color to tapped row in [Flutter DataTable](https://www.syncfusion.com/flutter-widgets/flutter-datagrid).

Initialize the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) widget with all the necessary properties. You can change the row background color using the [DataGridRowAdapter.color](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridRowAdapter/color.html) property. We used the [onCellTap](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid/onCellTap.html) callback to detect the rowIndex of the tapped row. Then, by using [DataGridSource.effectiveRows](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridSource/effectiveRows.html), you can retrieve the tapped row based on the rowIndex. To update the UI after a row is tapped, we called setState inside the onCellTap, which triggers the grid to rebuild and apply the background color to the tapped row.

```dart
@override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Syncfusion Flutter DataGrid'),
      ),
      body: SfDataGrid(
        source: employeeDataSource,
        onCellTap: (details) {
          // Ensure rowIndex is greater than 0 to avoid selecting the header.
          if (details.rowColumnIndex.rowIndex > 0) {
            // Adjust the row index to remove the header row from counting.
            setState(() {
              // When performing sorting or filtering,use DataGridSource.effectiveRows to retrieve the row.
              employeeDataSource.highlightRow = employeeDataSource
                  .effectiveRows[details.rowColumnIndex.rowIndex - 1];
            });
          }
        },
        columns: <GridColumn>[
          GridColumn(
              columnName: 'id',
              label: Container(
                  padding: const EdgeInsets.all(16.0),
                  alignment: Alignment.center,
                  child: const Text(
                    'ID',
                  ))),
          GridColumn(
              columnName: 'name',
              label: Container(
                  padding: const EdgeInsets.all(8.0),
                  alignment: Alignment.center,
                  child: const Text('Name'))),
          GridColumn(
              columnName: 'designation',
              label: Container(
                  padding: const EdgeInsets.all(8.0),
                  alignment: Alignment.center,
                  child: const Text(
                    'Designation',
                    overflow: TextOverflow.ellipsis,
                  ))),
          GridColumn(
              columnName: 'salary',
              label: Container(
                  padding: const EdgeInsets.all(8.0),
                  alignment: Alignment.center,
                  child: const Text('Salary'))),
        ],
      ),
    );
  }

class EmployeeDataSource extends DataGridSource {

  …..

  DataGridRow? highlightRow;

  @override
  DataGridRowAdapter buildRow(DataGridRow row) {
    // Function to determine the background color of a row.
    Color getBackgroundColor() {
      if (row == highlightRow) {
        return Colors.deepPurple;
      } else {
        return Colors.transparent;
      }
    }

    return DataGridRowAdapter(
        color: getBackgroundColor(),
        cells: row.getCells().map<Widget>((e) {
          return Container(
            alignment: Alignment.center,
            padding: const EdgeInsets.all(8.0),
            child: Text(e.value.toString()),
          );
        }).toList());
  }
}
```

You can download this example on [GitHub](https://github.com/SyncfusionExamples/How-to-add-background-color-to-tapped-row-in-Flutter-DataTable).