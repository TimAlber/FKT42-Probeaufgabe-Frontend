<script>
  export default {
  data() {
    return {
      data: null,
      showMirarbeiters: null,
      currentFimaId: null,
      currentFimaName: null,
    }
  },

  methods: {
    addMitarbeiter(){
      console.log(this.currentFimaId);
      if(this.currentFimaId == null){
        return;
      }

      let vorname = prompt('Geben sie den Vornamen ein.');
      let nachname = prompt('Geben sie den Nachnamen ein.');
      let email = prompt('Geben sie die Email ein.');

      if(vorname == null || nachname == null){
        alert('Name und Nachname sind Pflichtfeld.');
        return;
      }

      if(!email.match(/^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/)){
        alert('Email war ungültig. Erstelle Mitarbeiter erneut.');
        return;
      }

      var myHeaders = new Headers();
      myHeaders.append("Cookie", "XSRF-TOKEN=eyJpdiI6Ik5nTWtaVVJNYUZYUkFyaFByVGltMEE9PSIsInZhbHVlIjoieWpaUU4xMXFmbWdvTFcrZXB6NEtnZHAvbW15QjVIT0pIZkRKLzl0SGVBRzg4Q05wd2I0OWxScTQzOElzVTlnbHVidEdZNVRzWEdub3U3YW4zYm5NSVY1OEMrc2ZMZzhNTXQ0SEVJOWgzRkdLZ0QrTU5uNHgzSTN2dFpkVDlEK1YiLCJtYWMiOiIyOWRlODM5MmNlZTRjOTM1OTBkNDllMGE4NzY1ODE1ZWYxM2QwZDJiYjBhZDQ1MjNhN2MwOGY5MDFkZmViZGQ4IiwidGFnIjoiIn0%3D; laravel_session=eyJpdiI6IkJPdXd1bCtTUFFGNk1YeFBydDcrR2c9PSIsInZhbHVlIjoiYlYzS3ZNTy9LbkNiOXF5L2xkTytUU2pYNHJ5cEh1OFRpbjQzeUVCR2RYMU9zTStZcXBXMVdDalpQVHQrMVF5eDlHcFNuYUxZRnlYNmYxL0RaT2x1ck1DQmV3SWpGZG9wdVdQU0RDR0sxNG5hSk5qcllucTNpT2JtOXRjWGh2OHgiLCJtYWMiOiIxM2ExODNlM2EzOTQyMGY2MmJkNWQxNGFmOWNiZjI2MGQyMTRlZmVlY2VlN2JiNGI1NmY3Y2Y1NzUzZDU3MTkxIiwidGFnIjoiIn0%3D");

      var requestOptions = {
        method: 'GET',
        headers: myHeaders,
        redirect: 'follow'
      };

      fetch('http://localhost:8000/create-mitarbeiter?firmen_id='+this.currentFimaId+'&vorname='+vorname+'&nachname='+nachname+'&email='+email, requestOptions)
        .then(response => response.text())
        .then(result => {
          console.log(result);
          this.openfirma(this.currentFimaId, this.currentFimaName);
        })
        .catch(error => console.log('error', error));

    },
    openfirma(id, name) {
      console.log(id);
      this.currentFimaId = id;
      this.currentFimaName = name;

      var myHeaders = new Headers();
      myHeaders.append("Cookie", "XSRF-TOKEN=eyJpdiI6IjkveWw3MDhVSDhEVTdlZnpvU0E5MWc9PSIsInZhbHVlIjoiUy9LZW9uVFl1MjhYSVdSK1RSRDNMb2RLakVSd1AzeEJTYjN2VUNSUTk5UjFFZ2pvN2FIL3dld0ZXUTZkbDBMczJ1Zk9iNE5WcVQ3SFQyUlV6YnJUZmtjR1pudWpRenpoaDQzNjhKNGZnWjJrZ29XWCttRzA0QWU0b3J4YnRrTTEiLCJtYWMiOiI4Zjg5MGE4YzU4MjVhYzA0M2UzYmJkMjc5ZTE4YmVjYWU5N2FjMzYxY2MyOTUzMzFmNzRkMGFlYTllYjVlOGU4IiwidGFnIjoiIn0%3D; laravel_session=eyJpdiI6ImQ1SkxxNm5uV0hxOVBxb1grcWJhY2c9PSIsInZhbHVlIjoidkxXN2NNSzQ5YlQ0MHZVWWh6QjRLOHZlaEprbE1WOXdiNTBqdWNnNU5QeEhscXdYeERJbVJCVHRTNUg2UzQ2NFdjOEtxZ3ExOTI0TXZrM3k3b1YvdHNCOXV2OE82bW5VWjdIaHF1aTJJTEdaTVlQUDEvaGVuUmkyVUlJZXk0MnoiLCJtYWMiOiJiNTcyMzJiOGY3ODhkNmM1ZDQwNzkzNjMyNDE4ODJkYjc3ZjhjYWM0NWEyYzEwNDcxYTdiMjg2YTY5MDZiMjY5IiwidGFnIjoiIn0%3D");

      var requestOptions = {
        method: 'GET',
        headers: myHeaders,
        redirect: 'follow'
      };

      fetch("http://localhost:8000/get-mitarbeiter", requestOptions)
        .then(response => response.text())
        .then(result => {
          console.log(result);
          this.showMirarbeiters = JSON.parse(result).filter(mit => mit.firmen_id == id);
          if(this.showMirarbeiters.length == 0){
            this.showMirarbeiters = null;
          }
        })
        .catch(error => console.log('error', error));
    },
    addfirma() {
      let newName = prompt('Geben sie den Namen der neuen Frima ein.');
      console.log(newName);
      
      var myHeaders = new Headers();
      myHeaders.append("Cookie", "XSRF-TOKEN=eyJpdiI6IitLbVRnbnNwMW5oaFRsb1lkZGlPeFE9PSIsInZhbHVlIjoiYkE5d1Y4QUZWZnFkd3V0T05wNWxMWVFScHFkZ2J1Tmc2cjF4OGJMdG1jYmpkbVdySnFrTkZudUZleXRkOVRQdlN1VjErVjBtRm9NWVJ2c3ppZTRmZnplL1RjUVYrcTliRnJQeURaSWNZdUNYM00wSDdOcE5qbWNEV2FGTndzRngiLCJtYWMiOiIzZDNmNmE0NzE5OGIyZDE3YjRiMmQ2ZDk0YzVkZThmNmQxZGJiN2Y2OWRmYjM5ZDcwOGJlZDkwY2UyZTMxMTZjIiwidGFnIjoiIn0%3D; laravel_session=eyJpdiI6InNFT3BnbEhnT2E2dFJZMUcwdHJGenc9PSIsInZhbHVlIjoidEI2MTF6WStkSlB0ZjRjZFBSdmdUWHBtZlkrcFFMeU1zT1ppdHdnTmxnT0xuQ0xObmE4SEV5TFZQbk9WcUdyNGZEbWNsUTRGNlhPUGErMko4eFVCU2hsbkI2UVNQaWZCNEhTNGFsTS9nRDZFMm1NTjEvSFVid0ZNNktaQVZrdkQiLCJtYWMiOiJjYjBmNjY1MzYyY2RhNTY4NTVlM2VhNjZiMzhiZmNlZjAzYzc2YTI1NDQ4ZGU4NWJiMzdmMTkwZWZmYjhmYjBkIiwidGFnIjoiIn0%3D");

      var requestOptions = {
        method: 'GET',
        headers: myHeaders,
        redirect: 'follow'
      };

      fetch("http://localhost:8000/create-firma?firmenname="+newName, requestOptions)
        .then(response => response.text())
        .then(result => {
          console.log(result);
          this.reloadPage();
        })
        .catch(error => console.log('error', error));
      },
    reloadPage(){
        var myHeaders = new Headers();
        myHeaders.append("Cookie", "XSRF-TOKEN=eyJpdiI6IjQ0MUF3UWpwc0tLWHBBbzU5Nzh2K1E9PSIsInZhbHVlIjoiWUF0dDFYSmRSdldyNk1sc21PM2pBK1pUOTFnQy8wN1VWdTJRbEcxZWc0emJYaFdhRG1ZdUVTSjFFZjRuUGZjK2VRRUVGbVlwVlUzRjd4SE5BVDkwbXk5L085emVqNmIxZW5aYXhmZ0VFSWxxNkxKVEdUVVdzWGNnMjM4dGZCYlAiLCJtYWMiOiI4ODczZjE2YzE4MmMzNzFjMDBlYTRlMWIzMTE0OWQ3YTBmN2M0YzMzMDE4YWMxMTE0NDg0MTNkMmJmZTk5MmI4IiwidGFnIjoiIn0%3D; laravel_session=eyJpdiI6InNjUmtUWU04SllmNmJKZ3BrdTVrSnc9PSIsInZhbHVlIjoiQk5zdkpaWGYyYnY1bzNiZWZGQ3NzRUdITlg2RHVEWkdtdlQySXJVRUdheHFycWNFbE5SemczR2c3Nk8rNHRsRFI3a3NlZ256TG1DZ2lKTGdPU2d2aEhVNFZRM2QvN3pzZjFhcTBCVWdIQ3hKR09XdUVIQ0tYY3VzbXVtZ3NtdWsiLCJtYWMiOiI3NTA5MGEyNTJjYTEyNmJiNmNkNGY3ZGIxYzZhNzFiODBiODZjYjVhYjM4ZjUzNTQyNWFmYWQxODU2ZjNiZTM5IiwidGFnIjoiIn0%3D");

        var requestOptions = {
          method: 'GET',
          headers: myHeaders,
          redirect: 'follow'
        };

        fetch("http://localhost:8000/get-firma", requestOptions)
          .then(response => response.text())
          .then(result => {
            console.log(result);
            this.data = JSON.parse(result);
          })
          .catch(error => console.log('error', error));
    }
  },

  created() {
    this.reloadPage();
  }
}

</script>

<template>
  <button @click="addfirma">Frima Hinzufügen</button>

  <br>
  <table>
  <tr>
    <th>Nummer</th>
    <th>Frimenname</th>
    <th>Frima öffnen</th>
  </tr>
  <tr v-for="item in data">
    <td>{{ item.id }}</td>
    <td>{{ item.firmenname }}</td>
    <td>
      <button @click="openfirma(item.id, item.firmenname)">Frima öffnen</button>
    </td>
  </tr>
</table>

<br>
<div id="wrapper">

  <div v-if="currentFimaName != null">
    <p style="text-align:center">{{ currentFimaName }}:</p>
  </div>

  <div v-if="showMirarbeiters != null">
  <table>
  <tr>
    <th>Nummer</th>
    <th>Vorname</th>
    <th>Nachname</th>
    <th>Email</th>
  </tr>
  <tr v-for="item in showMirarbeiters">
    <td>{{ item.id }}</td>
    <td>{{ item.vorname }}</td>
    <td>{{ item.nachname }}</td>
    <td>{{ item.email }}</td>
  </tr>
</table>
</div>

<div v-if="currentFimaId != null">
  <button @click="addMitarbeiter(currentFimaId)">Mitarbeiter Hinzufügen</button>
</div>
</div>

</template>

<style scoped>
  button {
    font-weight: bold;
    width: 100%;
  }

  table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td, th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

#wrapper {
    float: left;
    width: 100%;
}
</style>
