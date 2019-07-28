# Version 1.0.0
# Description
A react data table component built on top of html table element.

# Features
* Pagination
   * Set initial page
   * Set records per page
   * Set records per page options
* Listen to table updates
* Search
* Sort
* Custom Cell Render
* Select Rows
   * Listen to selection changes
* Responsive
   * Collapsible Mode
   * Stacked Mode
   * Scrollable Mode
* Child Tables  

# Getting Started
Install:

npm:

```npm install euka-datatables```

yarn:

```yarn add euka-datatables```

# Usage

```javascript
import EukaDataTable from "euka-datatables";

let data = [
    {name:'Person X', contact:'0700000000', address:' P.0 Box 123456 00200'},
    {name:'Person Y', contact:'0711111111', address:' P.0 Box 654321 00300'}
];
let columns = [
    {
        name:'name',
        label:'Name'
    },
    {
        name:'contact',
        label:'Contact'
    },
    {
        name:'address',
        label:'Address'
    }
];
let title = 'Client List';//this can be a html element or react component
let options = {
    responsive:'collapse'
};

<EukaDataTable
    key={'table-1'}
    title={title}
    columns={columns}
    data={data}
    options={options}
/>
```

# Note
EukaDatatable has options for the table (```table level options```) as well as options for each column (```column level options```).
   
# Pagination
Pagination is a table level option named 'paginate'. By default pagination is enabled. 
Disable pagination by setting 'paginate' to false in table options. To set number of records per page, use table level 
option named 'recordsPerPage'. To provide a list of records per page for user to choose from, use table level option named 'recordsPerPageOptions'.

```javascript
let options = {//table options
    paginate:true,
    page:1,//initial page to be displayed
    recordsPerPage:5,//initial records per page. Default is 10
    recordsPerPageOptions:{5:5, 10:10, 20:20, All:-1}//if empty {} or null; the filter is not displayed
};
```

# Listen to Table Updates
You can listen to all table updates by using the table level option named 'onTableUpdate', whose value is a function that receives a currentInformation object containing common EukaDatatable updates.


```javascript
let options = {//table options
    onTableUpdate:(currentInformation)=>{
        //current info is an object containing common EukaDatatable updates
    }
};
```

The currentInformation object has below properties:

```
{
    searching:boolean,
    searchText:string,
    searchResultIndices:array,
    paginationInfo:object,
    data:array,
    sortingProperty:string,
    sortedAscending:boolean,
    sortResultIndices:array,
    selectionInfo:object
}
```

# Search
Search is a table level option named 'search'. By default search is enabled. 
Disable search by setting 'search' to false in table options.

```javascript
let options = {//table options
    search:true
};
```

# Sort
Sort is a column level option named 'sort'. By default sort is enable. 
Disable column sort by setting 'sort' to false in column options.

```javascript
let columns = [
    {
        name:'name',
        label:'Name'
    },
    {
        name:'contact',
        label:'Contact'
    },
    {
        name:'address',
        label:'Address',
        options:{//column options
           sort:true
        } 
    }
];
```

# Custom Cell Render
Custom cell render is a column level option named 'customCellRender'. 
To handle custom cell render set 'customCellRender' in column options.

```javascript
let columns = [
    {
        name:'name',
        label:'Name'
    },
    {
        name:'contact',
        label:'Contact'
    },
    {
        name:'address',
        label:'Address',
        options:{//column options
           customCellRender:(value, record)=>{
               //the initial 'value' before customization
               //the actual 'record' from the table data
               return record.contact+' ('+record.name+')'; //this can also be a html element or react component
           }
        } 
    }
];
```

# Select Rows
Selection of rows is a table level option named 'selectRows'. By default selection of rows is disabled. To enable selection set 'selectRows' to true in table options.
To listen to changes in the rows selected set 'onRowsSelect' in table options.

```javascript
let options = {//table options
    selectRows:true,
    onRowsSelect:(selectedDataIndices, selectedData)=>{
        //'selectedDataIndices' is an array of original indices of the selected records from the table data
        //'selectedData' is an array of the selected records from the table data
    }
};
```

# Responsiveness
Table responsiveness is a table level option named 'responsive'. Responsiveness has 3 modes:

```Collapsible:``` set 'responsive' to value 'collapse'. A table with collapse mode collapses and expands its columns according to the width of the table.
Each row has an icon that is clicked to show and hide its collapsed columns.

```javascript
let options = {//table options
    responsive:'collapse'
};
```

```Stacked:``` set 'responsive' to value 'stacked'.

```javascript
let options = {//table options
    responsive:'stacked'
};
```

```Scrollable:``` set 'responsive' to value 'scroll'.

```javascript
let options = {//table options
    responsive:'scroll'
};
```

# Child Tables
Child Table feature enables a row to show detailed information in tabular form. 
A row with a child table has an icon that is clicked to show and hide the child table accordingly.
Child table is a table level option. You can show child table for all rows or conditionally show for some rows.
A child table has similar features to that of a full table. Note that a child table cannot have a child table of its own.
The properties of a child table can be set to direct values or conditional values through functions. 
The functions receive a record of the clicked row to determine what value a certain property will hold. 
You can choose which properties will have direct values and which ones will have conditional values based on your requirements.

Declaring child property values directly:

```javascript
let options = {//table options
    childTable:{
        show:true, //to show or not to show the childTable. This is the only property that full tables do not have
        title:'Child Table',
        columns:columns,
        data:data,
        options:{
            paginate:true,
            recordsPerPage:2,
            recordsPerPageOptions:{},
            search:false,
            page:1
        }
    }
};
```

Declaring child property values through functions (conditional based values):

```javascript
let options = {//table options
    childTable:{
        show:(record)=>{return record.name !==''},
        title:(record)=>{return record.name},
        columns:(record)=>{return columns},
        data:(record)=>{return data},
        options:(record)=>{
            return {
                paginate:true,
                recordsPerPage:2,
                recordsPerPageOptions:{},
                search:false,
                page:1
            }
        }
    }
}
```
