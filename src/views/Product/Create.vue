<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-border">
                <h4>Add Product</h4>
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
                   <button type="button" @click="saveProduct()" class="btn btn-primary">Save</button>
                </div>
            </div>
        </div>

    </div>
</template>
<script>
import axios from 'axios'
export default {
    name:'productscreate',
    data(){
        return {
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
    methods:{
        saveProduct(){
            var mythis = this;
            axios.post('http://localhost:8000/api/products',this.model.product)
            .then(res => {
                alert(res.data.message);
              
                              
                this.model.product = {
                    name:'',
                    description:'',
                    price:''
                }
                this.errorList=''
                this.$router.push('/product');
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