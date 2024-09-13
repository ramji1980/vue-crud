<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-border">
                <h4>Edit Product</h4>
            </div>
            <div class="card-body">
                <ul class="alert alert-warning" v-if="Object.keys(errorList).length > 0">
                    <li class="mb-0 ms-3" v-for="(error, index) in this.errorList" :key="index">
                        {{ error[0] }}
                    </li>
                </ul>
                <div class="mb-3">
                    <label for="">
                        Name
                    </label>
                    <input type="text" v-model="model.product.name" class="form-control">
                </div>
                <div class="mb-3">
                    <label for="">
                        Description
                    </label>
                    <input type="text"  v-model="model.product.description" class="form-control">
                </div>
                <div class="mb-3">
                    <label for="">
                        Price
                    </label>
                    <input type="text"  v-model="model.product.price" class="form-control">
                </div>
                <div class="mb-3">
                   <button type="button" @click="updateProduct()" class="btn btn-primary">Update</button>
                </div>
            </div>
        </div>

    </div>
</template>
<script>
import axios from 'axios'
export default {
    name:'productsedit',
    data(){
        return {
            productId:'',
            errorList:'',
            model: {
                product:{
                    name:'',
                    description:'',
                    price:''
                }

            }
        }
    },
    mounted(){
        this.productId=this.$route.params.id;
        this.getPrdouct(this.$route.params.id)
    },
    methods:{
        getPrdouct(productId){
                axios.get('http://localhost:8000/api/products/'+productId).then(res=> {
                    this.products=res.data.data;
                  
                   this.model.product.name=res.data.data.product_name;
                   this.model.product.description=res.data.data.description;
                   this.model.product.price=res.data.data.price;
                  // this.model.product=this.products;
                });
            },
            
            updateProduct(){
            var mythis = this;
            axios.put(`http://localhost:8000/api/products/${this.productId}`,this.model.product)
            .then(res => {
                              
                alert(res.data.message);
                this.$router.push('/product');
                this.errorList=''
            })
            .catch(function(error) {
                if(error.response){
                    if (error.response.status == 422) {
                        mythis.errorList=error.response.data.errors;
                    }
                    
                } else if (error.request) {
                        console.log(error.request);
                } else {                    
                    console.log('Error', error.message);
                }
            
        })
    }
}
}

</script>