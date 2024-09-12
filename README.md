# memo



<app.jsx>

import 'bootstrap.css';

import '@mescius/wijmo.styles/wijmo.css';

import ReactDOM from 'react-dom/client';

import React from 'react';

import useEvent from 'react-use-event-hook';

import { FlexGrid, FlexGridColumn } from '@mescius/wijmo.react.grid';

import './app.css';

import { data } from './data';

function App() {

  const initializeGrid = useEvent((flex) => {

    flex.beginningEdit.addHandler((s, e) => {

      let item = e.getRow().dataItem, binding = e.getColumn().binding;

      //if (item.overdue && binding != 'overdue') { // prevent editing overdue items

      //e.cancel = true;

      //}

    });

    flex.cellEditEnded.addHandler((s, e) => {

​

      let col = s.columns[e.col];

      let row = s.rows[e.row];

      console.log("row:", row);

      console.log("e:", e);

      console.log("s:", s);

​

      if (col.binding == 'overdue' || col.binding == 'unoverdue') {

​

        let item = e.getRow().dataItem, binding = e.getColumn().binding;

        let v = item;

        console.log("1.item:", item);

​

        if (col.binding == 'overdue' && item.overdue == true) {

          item.unoverdue = !item.overdue;

          console.log("A.item.overdue:", item.overdue);

          console.log("A.item.unoverdue:", item.unoverdue);

        }

        if (col.binding == 'overdue' && item.overdue == false) {

          item.unoverdue = !item.overdue;

          console.log("B.item.overdue:", item.overdue);

          console.log("B.item.unoverdue:", item.unoverdue);

        }

        if (col.binding == 'unoverdue' && item.unoverdue == true) {

          item.overdue = !item.unoverdue;

          console.log("C.item.overdue:", item.overdue);

          console.log("C.item.unoverdue:", item.unoverdue);

        }

        if (col.binding == 'unoverdue' && item.unoverdue == false) {

          item.overdue = !item.unoverdue;

          console.log("D.item.overdue:", item.overdue);

          console.log("D.item.unoverdue:", item.unoverdue);

        }

        console.log("2.col.binding:", col.binding);

        console.log("3.item:", item);

        row._data = item;

        //e.cancel = true;

        flex.invalidate();

        //let value = wjcCore.changeType(s.activeEditor.value, wjcCore.DataType.Number, col.format);

        //if (!wjcCore.isNumber(value) || value < 0) { // prevent negative sales/expenses

        //  e.cancel = true;

        //  setLogText('Please enter a positive amount');

        //}

      }

​

    });

  });

  return (<div className="container-fluid">

    <FlexGrid initialized={initializeGrid} itemsSource={data}>

      <FlexGridColumn binding="id" header="ID" width={50} isReadOnly={true} />

      <FlexGridColumn binding="country" header="Country" isRequired={true} />

      <FlexGridColumn binding="sales" header="Sales" isRequired={false} />

      <FlexGridColumn binding="expenses" header="Expenses" isRequired={false} />

      <FlexGridColumn binding="overdue" header="Overdue" />

      <FlexGridColumn binding="unoverdue" header="unOverdue" />

    </FlexGrid>

  </div>);

}

const container = document.getElementById('app');

if (container) {

  const root = ReactDOM.createRoot(container);

  root.render(<App />);

}

<data.jsx>

function getData() {

    let countries = ['US', 'Germany', 'UK', 'Japan', 'Italy', 'Greece'], data = [];

    for (let i = 0; i < countries.length; i++) {

        data.push({

            id: i,

            country: countries[i],

            sales: Math.random() * 10000,

            expenses: Math.random() * 5000,

            overdue: i % 4 == 0,

            unoverdue: i % 4 != 0

        });

    }

    return data;

}

export const data = getData();

<app.css>

.wj-flexgrid {

  max-height: 200px;

  margin-bottom: 12px;

}

​

<index.html>

<!DOCTYPE html>

<html lang="en">

​

<head>

    <meta charset="utf-8">

    <meta http-equiv="X-UA-Compatible" content="IE=edge">

    <title>MESCIUS Wijmo Wijmo FlexGrid Read-only</title>

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

​

    <!-- SystemJS -->

    <script src="https://cdnjs.cloudflare.com/ajax/libs/systemjs/0.19.40/system.src.js" integrity="sha512-G6mEj6h18+m3MvzdviSDfPle/TfH0//cXcB33AKlNR/Rha0yQsKefDZKRTkIZos97HEGq2JMV1RT5ybMoQ3WsQ==" crossorigin="anonymous"></script>

    <script src="systemjs.config.js"></script>

    <script>

        System.import('./src/app');

    </script>

</head>

​

<body>

    <div id="app"></div>

</body>

​

</html>

​
