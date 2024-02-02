<template>
  <div id="invoice" class="container-fluid">
  <div class="header text-center">
    <h1>{{ companyName }}</h1>
    <p>{{ address }}</p>
  </div>
  <hr >
  <form>
    <h3 class="text-center text-info">{{ title }}</h3>
    <div class="row">
      <div class="col-md-3">
        <div class="form-group">
          <label for="invoiceNumber" class="text-bold">Invoice No</label>
          <input
           type="text" 
           id="invoiceNumber"
           class="form-control" 
           v-model="invoice.invoiceNo"
           />
        </div>
      </div>
      <div class="col-md-3">
        <div class="form-group">
          <label for="invoiceDate" class="text-bold">Invoice Date</label>
          <input
           type="text" 
           id="invoiceDate"
           class="form-control" 
           v-model="invoice.invoiceDate"
           />
        </div>
      </div>
      <div class="col-md-3">
        <div class="form-group">
          <label for="customer" class="text-bold">Customer</label>
         <span><small class="badge badge-info">{{custId}}</small></span>
          <select id="customer" v-model="invoice.custId" class="form-control">
            <option value="0" selected>-Select-</option>
            <option v-for ="c in customers" :value="c.id" :key="c.id">
            {{c.custName}}
            </option>
          </select>
        </div>
      </div>
      <div class="col-md-3">
        <div class="form-group">
          <label for="PODate" class="text-bold"> P O Date</label>
          <input
           type="text" 
           id="PODate"
           class="form-control" 
           v-model="invoice.poDate"
           />
        </div>
      </div>
    </div>
  </form>
  </div>
  <hr>
  <h5 class="text-center text-info">{{subTitle}}</h5>
<table class="table table-bordered table-striped table-hover">
  <thead>
    <tr class="text-center">
      <th>Product</th>
      <th>Unit Price</th>
      <th>Quantity</th>
      <th>Discount %</th>
      <th>Sub-Total</th>
      <th>Tax %</th>
      <th>Tax Value</th>
      <th>Total</th>
      <th>
        <button type="button" class="btn btn-success btn-sm d-print-none" @click="addItem">
          <span area-hidden="true"> &plus;</span>
        </button>
      </th>
    </tr>
  </thead>
  <tbody v-for="(d,i) in invoice.invoiceDetails " :key="i">
<tr>
  <td width="20%">
    <div class="form-group">
      <select id="products" v-model="d.productId" class="form-contrl">
        <option value="0">-Select -</option>
        <option v-for="p in products" :value="p.id" :key=p.id>{{p.productName}} </option>
 
      </select>
    </div>
  </td>
    <td width="10%">
    <div class="form-group">
      <input type="text" id="unitPrice" v-model="d.unitPrice" class="form-control">
   
   
    </div>
  </td>
  <td width="10%">
<div class="form-group">
<input type="qty" v-model="d.qty" class="form-control">

</div>
  
   </td>
   <td width="10%">
<div class="form-group">
<input type="discountRate" v-model="d.discountRate" class="form-control">

</div>

</td>
<td width="12%">
  <div class="form-group">
    <span>{{d.subTotal=(d.unitPrice*d.qty*(1-d.discountRate/100)).toFixed(2)}}</span>
 
  </div>

</td>
 <td width="10%">
<div class="form-group">
<input id="taxrate" v-model="d.taxRate" class="form-control">

</div>

</td>
 <td width="10%">
 <div class="form-group">
  <span>{{d.taxValue=(parseFloat(d.subTotal)*(d.taxRate/100).toFixed(2))}}</span>

</div>

</td>
<td width="10%">
<div class="form-group">
<span>{{d.total=(parseFloat(d.subTotal)+parseFloat(d.taxValue)).toFixed(2)}}</span>

</div>

</td>
<td width="10%">
<div class="form-group">

  <button type="button" class="btn btn-danger btn-sm d-print-none" @click="removeItem(i)">
   <span area-hidden="true"> &minus;</span>
   </button>

</div> 

</td>
</tr>

  </tbody>
<tfoot>
<tr>
  <td>
    <strong>Items</strong> &Sigma;{{productCount}}
  </td>
<td colspan="3" class="text-center">
  <strong>Total:</strong>
</td>
          <td>
            <span class="text-info text-bold total-underline">{{getSubTot}}</span>
          </td>
          <td></td>
           <td>
            <span class="text-info text-bold total-underline">{{getTaxTot}}</span>
          </td>
          <td>
            <span class="text-info text-bold total-underline">{{getTot}}</span>
          </td>
        </tr>

</tfoot>

</table>
<div class="invoice-footer text-center">
  <small>Thank You for choosing us!</small>
</div>
</template>

<script>
export default {
  name: 'App',
  methods: {
    addItem(){
      let newItem = {
        id:0,
        parentInvoiceId:0,
        productId:0,
        unitPrice:0,
        qty:0,
        discountRate:0,
        subTotal:0,
        taxRate:0,
        taxValue:0,
        total:0,
        margin:0,
      };
      this.invoice.invoiceDetails.push(newItem);
    },
    removeItem(id){
      let isConfirms=confirm("Do you want to delete data")
      if(isConfirms){
        this.invoice.invoiceDetails.splice(id,1);

      }
    
  }
},

  data(){
    return{
      companyName:"Shubham Motors",
      address:"www.shubhammotors.in",
      title:"Sales Invoice",
      subTitle:"Invoice Details",
      invoice:{
        invoiceNo:'inv-001',
        invoiceDate:"29-01-2024",
        custId:0,
        poDate:"31-01-2024",
        invoiceDetails:[
            {
              id:0,
              parentInvoiceId:0,
              poductId:0,
              unitPrice:0,
              qty:0,
              discountRate:0,
              subTotal:0,
              taxRate:0,
              taxValue:0,
              total:0,
              margin:0
            }
        ]
      },
      customers:[
        {id:1,custName:"Customer 1"},
        {id:2,custName:"Customer 2"},
        {id:3,custName:"Customer 3"},
        {id:4,custName:"Customer 4"},
        {id:5,custName:"Customer 5"},
        {id:6,custName:"Customer 6"},
        {id:7,custName:"Customer 7"},
        {id:8,custName:"Customer 8"},
      ],
      products:[
        {id:1,productName:"product 1"},
        {id:2,productName:"product 2"},
        {id:3,productName:"product 3"},
        {id:4,productName:"product 4"},
        {id:5,productName:"product 5"},
        {id:6,productName:"product 6"},
        {id:7,productName:"product 7"},
        {id:8,productName:"product 8"},
      ],
   };
  },
  computed:{
    productCount(){
      let i=this.invoice.invoiceDetails.length-1
      let val=0;
      for(i;i>=0;i--){
        val+=parseFloat(this.invoice.invoiceDetails[i].qty)
      }

      return val
    },
    getSubTot(){
      let i=this.invoice.invoiceDetails.length-1
      let val=0;
      for(i;i>=0;i--){
        val+=parseFloat(this.invoice.invoiceDetails[i].subTotal)
      }

      return val
    },
    getTaxTot(){
        let i=this.invoice.invoiceDetails.length-1
      let val=0;
      for(i;i>=0;i--){
        val+=parseFloat(this.invoice.invoiceDetails[i].taxValue)
      }

      return val
    },
    getTot(){
      let i=this.invoice.invoiceDetails.length-1
      let val=0;
      for(i;i>=0;i--){
        val+=parseFloat(this.invoice.invoiceDetails[i].total)
      }

      return val
    }
   
  }
};
</script>

<style>
body {
  margin: 6px;
}
.header {
  width: 100%;
  margin: 0%;
  background: rgb(52, 52, 122);
  padding: 9px;
  color: aliceblue;
}
.header h1 {
  font-size: 3.3em;
}
hr{
  border-top: 2px solid #008b8b;
}
.text-bold{
  font-weight: 5000;
}
invoice-footer{
  width:100%;
  margin: 0 auto;
  background: #008b8b;
  padding: 6px;
  color:#fff;
}
@media print {
  .d-print-none 
  {
    display: none!important;
  }
}
</style>
