<template>
    <Header3 />
        <div id="container">
            <p id="presentation" >Bonjour {{user.nom}} {{user.prenom}}</p>  
            <p id="message_accueil"> Messages à consulter ! </p>
            <ul id="list" >
                <li v-for="message in messages " :key="message.id" id="contour_message"   >  
                    <div class="list_content"> 
                        <div id="align_message"> 
                                <p class="margeG2 titre_center"><span >Titre:</span>{{message.titre}}</p>
                                <p class=" margeG1 "><span>Message :</span>{{message.contenu}}</p>
                            <br />
                            <div id="align_nom_date">   
                                <p class="margeD">{{message.User.nom}}  {{ message.User.prenom}}</p>  
                                <p class="margeD">{{message.createdAt}}</p>                        
                            </div> 
                            
                        </div>
                        <div >
                            <p><img :src="message.imageUrl" id="message_image" alt=""/></p>
                        </div>
                    </div>
                    <div class="boutons">
                        <input @click="()=> deleteMessage(message.id)"  type="submit" class="btn"   value="Supprimer Message" /> 
                    </div>
                    <hr id="hr"> 
                </li >
            </ul>
        </div>
    <Footer />  
</template>
<script>
//import moment from 'moment';
import Header3 from '@/components/Header3.vue'
import Footer from '@/components/Footer.vue'
import axios from 'axios';
export default {
    name: 'DisplayMessages',
    components: {
      Header3, Footer
    },
    data() { 
        return { 
            messages : "",
           user: "" , 
        }
    },
    mounted(){ 
        /*const createdAt
        moment(createdAt).format('LLL')
        .then(res => {
                    const dateCreation = res.dateCreation;
                    this.message.createdAt = dateCreation;
            });*/
      
        const config = {
        headers: { Authorization: "Bearer " + localStorage.token } 
        }
        axios.get("http://localhost:3000/api/messages/display", config )
            .then(res => {
                    const data = res.data;
                    this.messages = data;
            });

        axios.get("http://localhost:3000/api/users/me/" + this.user.id ,config )
            .then(res => {
                    const data = res.data;
                    this.user = data;
            });
    },
    
    methods: {
        detailMessage(){
            const config = {
                headers: { Authorization: "Bearer " + localStorage.token } 
                }
            axios.get("http://localhost:3000/api/messages/display/" + this.messageId , config )
            .then ((response) => {
                this.response =response.data; 
                this.$router.push({ path: "/DisplayMessages" });
                console.log(response.data);
            })
           
        },
        deleteMessage(messageId){
            const config = {
                headers: { Authorization: "Bearer " + localStorage.token } 
                }
            axios.delete("http://localhost:3000/api/messages/delete/" + messageId, config )
            .then(res => {
                const data = res.data;
                this.message = data;
                window.location.reload();
            })
            .catch(function()  {
            window.alert('Vous ne pouvez pas supprimer ce message!');
            this.$router.push({query: {alert:'Vous ne pouvez pas supprimer ce message!'} });
            });
        },
    }
}

</script>
<style>
#container {
    background-color: #ffd9d9;
    margin-bottom: 0;
    margin-top: 0;
}
#message_accueil {
    color: #f57a54;
    font-size: 25px;
}
#presentation {
    font-size: 25px;
    font-weight: bold;
    padding-top: 30px;
}
.titre_center{
    text-align: center;
}
#list {
    list-style-type: none;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
}
.list_content {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    box-shadow: 5px 5px 8px #bb581e, -5px -5px 8px #bb581e;
    border-radius: 3%;
}
span {
    color: red;
}
.margeD {
    margin:  0px 15px 0 0px;
    font-size: 12px;
    font-weight: bold;
    text-align: right;
}
.margeG1 {
    margin:  0px 0px 0px 15px;
    font-size: 20px;
    text-align: left;
}
.margeG2 {
    margin:  0px 100px 0px 100px;
    font-size: 20px;
    text-align: left;
    padding-top: 0;
}
#global {
    max-width: auto;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
}
#align_nom_date {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}
.btn {
  border: 0;
  line-height: 2;
  padding: 0 20px;
  margin-top: 20px;
  font-size: 10px;
  text-align: center;
  border-radius: 10px;
  background-color: #F4511E;
  text-decoration: none;
}
#message_image {
    width: 100%;
    height: 100%;
}

#align_message{
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    width: 350px;
    height: 170px;
    background-color: cornflowerblue;
}
#hr {
  height: 15px;
  background-color:  black;
  border: none;
}
#align_phot{
    background-color: cornflowerblue;
}
</style>