# Version 1.2.4

# Description
A react data table component built on top of html table element.

[![Edit euka-datatables](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/euka-datatables-7ll3n?fontsize=14)

# Features
* Pagination
   * Set initial page
   * Set records per page
   * Set records per page options
* Listen to table updates
* Listen to row and cell click
* Search
* Sort
* Custom Cell Render
* Select Rows
   * Listen to selection changes 
* Responsiveness
   * Collapsible Mode
   * Stacked Mode
   * Scrollable Mode
* Child Tables  
* Language Settings
* Server Side Handling
* Footer Rows
* Styling

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

# Listen to Row and Cell Click
You can listen to row click by using the table level option named 'onRowClick'.

```javascript
let options = {//table options
    onRowClick:(rowInformation)=>{
        
    }
};
```

The rowInformation has below properties:

```
{
  rowIndex:number,
  dataIndex:number,
  record:object
}
```

You can listen to cell click by using the table level option named 'onCellClick'.

```javascript
let options = {//table options
    onCellClick:(cellInformation)=>{
        
    }
};
```

The cellInformation has below properties:

```
{
  columnIndex:number,
  rowIndex:number,
  dataIndex:number,
  cellValue,
  column:object,
  record:object
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

To set the initial sort variables already applied to the data, use table level options.
        
```javascript
let options = {//table options
    sortingProperty:'', //column name used to sort the data
    sortingColumnIndex:-1, //index of column used to sort the data
    sortedAscending:true  // set to false if the data is sorted in descending order
};
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

By default, EukaDataTables collapses columns from the last towards the first. To change this behaviour, use a column level numeric option named 'responsivePriority'.
The column with the least 'responsivePriority' value is collapsed last.

```javascript
let options = {//column options
    responsivePriority:-1// this value can be negative or positive
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

# Language Settings
Language is a table level option named 'language'. Language option allows the setting of common EukaDataTable phrases.
To provide custom phrases, use table level option named 'language'.

```javascript
let options = {//table options
    language:{//the default language settings are as listed
        emptyTable:'No records found',
        zeroRecordsOnFilter:'No matching records found',
        search:'Search:',
        recordsPerPage:'Records per page:',
        paginationInfo:'Showing _START_ to _END_ of _TOTAL_ records',//The key words _START_, _END_ and _TOTAL_ must be in your custom phrase for replacement with their corresponding numeric values
        paginationInfoFiltered:'filtered from _MAX_ records',//The key word _MAX_ must be in your custom phrase for replacement with its corresponding numeric value
        pages:'_TOTAL_ pages',//The key word _TOTAL_ must be in your custom phrase for replacement with its corresponding numeric value
        thousandSeparator:','
    }
};
```

# Server Side Handling

Server Side Handling is a table level option named 'serverSide'. Server Side option allows the handling of required data changes from server end. The changes include getting data required on pagination, on searching 
and on changing records-per-page. To use 'serverSide' option, prepare your server to serve paginated data.

```javascript
let options = {//table options
    serverSide:{//the default serverSide settings are as listed
        enable:false,//setting this to true enables serverSide feature
        totalRecords:dataLength,//the total records available in the server
        showFilteredFrom:false,//to show the 'filtered from phrase' set this to true . 
        maxRecords:dataLength//the maximum records from which data is filtered; used in 'filtered from phrase'. NOTE: If not interested with the 'filtered from phrase', you can ignore the maxRecords value
    }
};
```

To handle serverSide requirements at runtime you need to register 'onTableUpdate' which is a table level option.

```javascript
let options = {//table options
    onTableUpdate:(currentInformation)=>{//the important items for server side requests are as determined below from currentInformation object of onTableChange listener
        let {currentPage, recordsPerPage} = currentInformation.paginationInfo;
        let {searching, searchText, sortingProperty, sortingColumnIndex, sortedAscending} = currentInformation;
        this.fetchData({page:currentPage, recordsPerPage, searching, searchText, sortingProperty, sortingColumnIndex, sortedAscending});//fetchData is just a sample custom function that should fetch data from server on table change.
    }
};
```

If the sample fetchData function returns a success response, update the EukaDataTable with the currentPage, data, totalRecords and maxRecords from the server:

```javascript
let options = {...this.state.options};
options.page=currentPage;//the currentPage from the onTableUpdate listener; which can also be a part of the server response
options.serverSide.totalRecords = totalRecords; //totalRecords being a value from the server response
options.serverSide.maxRecords = maxRecords;//maxRecords being a value from the server response. NOTE: If not interested with the 'filtered from phrase', you can ignore the maxRecords value
let data = newData;//newData being the data from the server response
this.setState({data, options});//update the state that manages EukaDataTable to reflect the server response.
```

# Footer Rows

Footer rows stick at the bottom of the table irregardless of table changes. To set a table row, a table level option named 'footerRows' is used. Note that footer rows follow the same declaration as any other row in terms of column naming.
When collapsed, the cells of a footer row do not display column name like the cells of ordinary rows do.

```javascript
let footerRows = [{name:'100', contact:'200', address:'300'}];//the column naming is similar to that of ordinary rows
let options = {//table options
    footerRows:footerRows
};
```

# Styling

Create a css file with custom styling as shown below. Import the file in the class that renders EukaDataTable.

```css
.euka-datatables .datatable-wrapper {
    background: white;
    color: black;
}
.euka-datatables .pagination button.active {
    background: #4DAF50!important;
    color: white;
}
.euka-datatables tr.footer-row{
    font-weight: bold!important;
}
.euka-datatables tr.footer-row td{
    border-top: 2px solid black!important;
}
```



