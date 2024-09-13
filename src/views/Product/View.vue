<template>
   <div class="container">
    <div class="card">
        <div class="card-header">
            <h4>
                Products
                <RouterLink to="/product/create" class="btn btn-primary float-end">Add Product</RouterLink>
            </h4>
        </div>
        <div class="card-bod">
            <table class="table table-bordered">
                <thead>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Description</th>
                    <th>Price</th>
                    <th>Created at</th>
                    <th>Actions</th>
                </thead>
                <tbody v-if="this.products.length > 0">
                <tr v-for="(product,index) in this.products" :key="index">
                    <td>{{ product.id }}</td>
                    <td>{{ product.product_name }}</td>
                    <td>{{ product.description }}</td>
                    <td>{{ product.price }}</td>
                    <td>{{ product.created_at }}</td>
                    <td>
                        <RouterLink :to="{ path:'/api/products/'+product.id}" class="btn btn-success">
                            Edit 
                        </RouterLink>
                        |
                        <button type="button" @click="deleteProduct(product.id)" class="btn btn-danger">
                            Delete 
                        </button>
                    </td>
                </tr>
            </tbody>
            <tbody v-else>
                <tr>
                    <td colspan="7"> Loadding...</td>
                </tr>
               
            </tbody>
            </table>
        </div>
    </div>
   </div>
  </template>
  <script>
  import axios from 'axios'
    export  default {
        name:'products',
        data(){
            return {
                products:[]
            }
        },
        mounted(){
            this.getPrdoucts();
        },
        methods:{
            getPrdoucts(){
                axios.get('http://localhost:8000/api/products').then(res=> {
                    this.products=res.data.data;
                   
                });
            },
            deleteProduct(id){
                
                if(confirm('Are you sure you want to delete this product?')){
                    console.log(id);
                     axios.delete(`http://localhost:8000/api/products/${id}`)
                     .then(res=> {
                            alert(res.data.message);     
                            this.getPrdoucts();              
                     });
                }
                
            },
            
        }
    }
</script>
  
  
  