<template>

    <div >
        <v-card id="addCard">
            <v-row>
                <v-col cols="12" sm="6" lg="4" class="mx-auto" >
                    <v-text-field v-model="newDish.dishType" label="Type rett"></v-text-field>
                    <v-text-field v-model="newDish.dishName" label="Navn på rett"></v-text-field>
                    <v-text-field v-model="newDish.ingredients" label="Legg til ingredienser"></v-text-field>
                    <v-text-field v-model="newDish.vegetarian" label="Vegetar?"></v-text-field>
                    <v-text-field v-model="newDish.allergens" label="Legg til allergener"></v-text-field>
                    <v-text-field v-model.number="newDish.price" type="number" label="Legg til pris"></v-text-field>
                    <v-file-input v-model="file" label="Legg til bilde" show-size></v-file-input>
                    <v-btn @click="postDish">Lagre ny rett</v-btn>
                </v-col>
            </v-row>
        </v-card>
    </div>

</template>
<script>
import axios from 'axios'
export default {
    name: "DishForm",
    data(){
        return {
            newDish: { dishType: "", dishName: "", ingredients: "", allergens:"",  imageSrc: "" },
            file: null
        }
    },
    methods: {
        //Method to create a new entry in the database with the info in the text fields
        postDish(){
            this.newDish.imageSrc = this.newDish.dishName+".jpg";

                let data = new FormData();
                data.append("file", this.file);

                axios.post("https://localhost:5001/meny", this.newDish).then( result => {
                        console.log( result.data );

                        axios({
                            method: "POST",
                            url: "https://localhost:5001/menyadmin/uploadimage",
                            data: data,
                            config: { headers: { 'Content-Type': 'multipart/form-data' } }       
                        })

                })
            }
        }
    }
</script>

<style lang="scss">
#addCard{ 
    
    background-color: #fff;
    flex: 0 0 auto; margin-right: 30%; margin-top: 20px; margin-left:30%;
    box-shadow: 0 4px 8px 0 rgba($color: #000000, $alpha: 1.0);
    overflow: hidden;

}
</style>