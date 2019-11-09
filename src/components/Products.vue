<template>
 <div class="container">
      <v-client-table 
          :data="tableData"
          :columns="columns" 
          :options="options">
      </v-client-table>
  </div>
</template>

<script>
import db from '@/db'
export default {
  name: 'Products',
  data () {
   return {
            columns: ['id', 'name', 'price', 'action'],
            tableData: [],
            options: {
                headings: {
                    id: 'ID',
                    name: 'Name',
                    price: 'Price',
                    action: 'Action'

                },
                sortable: ['name', 'price'],
                filterable: ['name', 'price']
            }
        }
    },





    mounted() {
        this.axios.get('http://localhost:4000/prods').then(res => {
            this.tableData = res.data
        })
  },

 
  methods: {

    onSubmit(){
      db.collection('products').add(this.productData).then(this.getProducts)
      this.productData.product_id = ''
      this.productData.product_name = ''
      this.productData.product_price = ''

    },
    // onDelete(product_id){
    //   db.collection('products').where('product_id', '==', product_id).get().then(querySnapshot =>{
    //     querySnapshot.forEach(doc=>{
    //       doc.ref.delete().then(this.getProducts)
    //     })
    //   })
    // }
    onDelete(id){
      db.collection('products').doc(id).delete().then((data)=> {
          this.getProducts()
      })
    },
    onEdit(product){
      this.editId = product.id
      this.editProductData.product_id = product.product_id
      this.editProductData.product_name = product.product_name
      this.editProductData.product_price = product.product_price
    },
    onCancel(){
      this.editId = ''
      this.editProductData.product_id = ''
      this.editProductData.product_name = ''
      this.editProductData.product_price = ''
    },
    onEditSubmit (id){
      db.collection("products").doc(id).set(this.editProductData).then(
        this.getProducts)
        this.editId = ''
        this.editProductData.product_id = ''
        this.editProductData.product_name = ''
        this.editProductData.product_price = ''
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3{
  text-align: center;
  margin-top: 30px;
  margin-bottom: 20px;
}
.icon{
  margin-right: 10px;
}
.icon i{
  cursor: pointer;
}
</style>
