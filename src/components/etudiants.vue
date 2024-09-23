<template lang="html">
    <div>
      <div class="form-container">
        <form v-on:submit.prevent="addOrUpdateItem">
          <div style="display: inline-block;">
            First Name : <input v-model="firstName" required placeholder="Enter first name"><br><br>
            Last Name : <input v-model="lastName" required placeholder="Enter last name"><br><br>
            <input v-model="note1_1" hidden>
            <input v-model="note2_2" hidden>
            <input v-model="note3_3" hidden >
            <input v-model="moyenne_1" hidden >
            <button type="submit">Valider</button>
          </div>
        </form>
      </div>
      <div class="form-container2">
        <form v-on:submit.prevent="addOrUpdateNote">
          <div style="display: inline-block;">
            
            ID :  <input v-model="ID"  readonly disabled><br><br>
            Note 1 :  <input v-model="note1"  placeholder="Note 1"><br><br>
            Note 2 :  <input v-model="note2"  placeholder="Note 2"><br><br>
            Note 3 :  <input v-model="note3"  placeholder="Note 3"><br><br>
            <div style="height: 0px;">
                
            <input  v-model="firstName2"  hidden><br><br>
            <input  v-model="lastName2" hidden><br><br>
            </div>
            <button type="submit">Valider</button>
          </div>
        </form>
      </div>
      <br><br>
      <br><br>
      <hr>
      <center>
        <h1>Liste des Etudiants</h1>
        <table>
          <tr>
            <th>ID</th>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Notes</th>
            <th>Moyenne</th>
            <th><button @click="Ajouter">Ajouter</button></th>
          </tr>
          <tr v-for="etudiant in etudiants" :key="etudiant.id">
            <td>{{etudiant.id}}</td>
            <td>{{etudiant.firstName}}</td>
            <td>{{etudiant.lastName}}</td>
            <td>{{etudiant.note1}} - {{etudiant.note2}} - {{etudiant.note3}}</td>
            <td>{{etudiant.moyenne}}</td>
            <td>
              <button @click="editEtudiant(etudiant)" class="edit">Edit</button>
              <button @click="deleteEtudiant(etudiant.id)" class="delete">Delete</button>
              <button @click="notes(etudiant)" class="notes">Notes</button> 
            </td>
          </tr>
        </table>
      </center>
    </div>
  </template>
  
  <script>
  export default {
    name: "FirstNameLastName",
    data() {
      return {
        firstName: null,
        lastName: null,
        note1: null,
        note2: null,
        note3: null,
        moyenne: null,
        methode: "ajouter",
        etudiants: []
      }
    },
    methods: {
      addOrUpdateItem() {
        if (this.methode == "ajouter") {
          let item = {
            id: this.etudiants.length + 1,
            firstName: this.firstName,
            lastName: this.lastName,
            note1:this.note1,
            note2:this.note2,
            note3:this.note3,
            moyenne:this.moyenne,
          }
          this.etudiants.push(item)
          this.firstName = null
          this.lastName = null
        } else {
          let item = {
            id: this.id,
            firstName: this.firstName,
            lastName: this.lastName,
            note1: this.note1_1,
            note2: this.note2_2,
            note3: this.note3_3,
            moyenne: this.moyenne_1
          }
          this.etudiants = this.etudiants.map((etudiant) => etudiant.id === item.id ? item : etudiant)
          this.firstName = null
          this.lastName = null
          this.methode = "ajouter"
        }
      },
      addOrUpdateNote() {
        let note1 = this.note1 ? parseInt(this.note1) : 0;
        let note2 = this.note2 ? parseInt(this.note2) : 0;
        let note3 = this.note3 ? parseInt(this.note3) : 0;
        let item = {
            id: this.ID,
            note1: note1,
            note2: note2,
            note3: note3,
            moyenne: ((note1 + note2 + note3) / 3).toFixed(2),
            firstName: this.firstName2,
            lastName: this.lastName2
          }
          this.etudiants = this.etudiants.map((etudiant) => etudiant.id === item.id ? item : etudiant)
        this.ID = null;
        this.note1 = null;
        this.note2 = null;
        this.note3 = null;
      },
      Ajouter() {
        this.firstName = ""
        this.lastName = ""
        this.methode = "ajouter"
      },
      editEtudiant(etudiant) {
        this.firstName = etudiant.firstName;
        this.lastName = etudiant.lastName;
        this.id = etudiant.id;
        this.note1_1 = etudiant.note1;
        this.note2_2 = etudiant.note2;
        this.note3_3 = etudiant.note3;
        this.moyenne_1 = etudiant.moyenne
        this.methode = "edit"
      },
      deleteEtudiant(id) {
        if (window.confirm('Are you sure you want to delete this student?')) {
          this.etudiants = this.etudiants.filter((etudiant) => etudiant.id !== id);
        }
      },
      moyenneN(){

      },
      notes(etudiant) {
        this.ID = etudiant.id;
        this.note1 = etudiant.note1;
        this.note2 = etudiant.note2;
        this.note3 = etudiant.note3;
        this.firstName2 = etudiant.firstName;
        this.lastName2 = etudiant.lastName;
        if(etudiant.note1 == null)this.note1 = "";
        if(etudiant.note2 == null)this.note2 = "";
        if(etudiant.note3 == null)this.note3 = "";
      }
    }
  }
  </script>
  
  <style scoped>
  .form-container, .form-container2 {
    display: inline-block;
    vertical-align: top;
    width: 500px;
    margin: 0 5px;
    margin-left : 10%;
    margin-top : -3%;
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }
  .form-container {
    margin-left : 2%;

  }
  
  input {
    width: calc(100% - 22px);
    padding: 5px;
    margin: 5px 0;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-sizing: border-box;
  }
  
  button {
    padding: 5px 15px;
    margin: 10px 0;
    margin-left: 10px ;
    border: none;
    border-radius: 5px;
    background-color: #4CAF50;
    color: white;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  button:hover {
    background-color: #45a049;
  }
  
  button:focus {
    outline: none;
  }
  
  table {
    width: 80%;
    border-collapse: collapse;
    margin-top: 20px;
  }
  
  th, td {
    border: 1px solid #ccc;
    padding: 4px;
    text-align: center;
  }
  
  th {
    background-color: #f2f2f2;
  }
  
  .edit {
    background-color: #2196F3;
    color: white;
  }
  
  .delete {
    background-color: #f44336;
    color: white;
  }
  
  .notes {
    background-color: #977e38;
    color: white;
  }
  </style>
  