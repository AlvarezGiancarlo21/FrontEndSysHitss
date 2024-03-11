<template >
    <v-card class="mx-auto card" style="background-color: aquamarine;"
      width="1100px"
      height="500px"
      >

      <v-card-title class="d-flex align-center pe-2">
        <div class="claseCabeceraTabla">
          <v-icon icon="mdi-tools"></v-icon> &nbsp;
        Servicios Global Hitss
        </div>

    <v-row justify="center" align="center">
      <v-dialog
        v-model="dialog"
        persistent
        style="width: 850px;height: 1000px;"
      >
        <template v-slot:activator="{ props }">
          <v-btn
            v-bind="props"
            style="background-color: #05a2a5;color: aliceblue;"
          >
            Nuevo Cliente
          </v-btn>
        </template>
        <v-card>
          <v-card-title>
            <br>
            <h1 class="text-h5 tituloNS">Datos del cliente que se le brindara servicio</h1>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col
                  cols="12"
                  sm="6"
                  md="3"
                >
                  <v-text-field
                    label="Nombre de Cliente"
                    variant="outlined"
                    required
                  ></v-text-field>
                </v-col>
                <v-col
                  cols="12"
                  sm="2"
                  md="3"
                >
                  <v-select
                    :items="['Activo', 'En Proceso', 'Inactivo']"
                    label="Status"
                    variant="outlined"
                    required
                  ></v-select>
                </v-col>
                <v-col
                  cols="12"
                  sm="6"
                  md="3"
                >
                  <v-select
                    :items="['A) Nueva Visita', 'B) Oportunidad', 'C) Diseño de Solución','D) PoC','E) Desarrollo de propuesta','F) Propuesta Entregada']"
                    label="Avance"
                    variant="outlined"
                    required
                  ></v-select>
                </v-col>
                <v-col
                  cols="12"
                  sm="6"
                  md="3"
                >
                  <v-text-field
                    label="Fecha"
                    variant="outlined"
                    required
                  ></v-text-field>
                </v-col>

                <v-col
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-select
                    :items="['Pilar Bedoya', 'Anabella Scull', 'Victor Leonardo','Miguel Sardon','Hugo Zarate','Rogger Risco','Sandra Cartagena']"
                    label="Consultor"
                    variant="outlined"
                    required
                  ></v-select>
                </v-col>


                <v-col
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-select
                    :items="['DataLake DlaaS', 'Est. Trabajo Remoto', 'SW Factory','Servicios de nube','SW Factory','Power BI','Mesa de ayuda','Video Analitica','RPA','IoT']"
                    label="Soluciones/Servicios"
                    variant="outlined"
                    required
                  ></v-select>
                </v-col>
                <v-col
                  cols="12"
                  sm="6"
                  md="4"
                >
                  <v-select
                    :items="['Google', 'Global Hitss', 'Awingu','Microsoft','Digevo','Huawei','Automation Anywhere','Service Now','Sin Alianza']"
                    label="Alianza"
                    variant="outlined"
                    required
                  ></v-select>
                </v-col>
                <v-col
                  cols="12"
                  sm="6"
                  md="8"
                  style="height: 10px;"
                >
                <v-textarea
                  clearable
                  clear-icon="mdi-close-circle"
                  label="Descripcion"
                  variant="outlined"
                ></v-textarea>
                </v-col>
                <div style="justify-content: center;">


                <v-col cols="12" sm="6" md="2" >

                    <v-btn
                    size="x-large"
                    variant="text"
                    @click="dialog = false"
                    >Close</v-btn>




                    <v-btn
                    size="x-large"
                    variant="text"
                    @click="dialog = false"
                    color="blue"
                    >Crear</v-btn>
                </v-col>
              </div>

              </v-row>

            </v-container>

          </v-card-text>
        </v-card>
      </v-dialog>
    </v-row>
    <v-row justify="center" align="center">
        <v-btn style="background-color: #05a2a5;color: aliceblue;" @click="exportToExcel">
          Exportar a Excel
        </v-btn>
      </v-row>
        <v-spacer></v-spacer>

        <v-text-field
          v-model="search"
          prepend-inner-icon="mdi-magnify"
          density="compact"
          label="Search"
          single-line
          flat
          hide-details
          variant="solo-filled"
        ></v-text-field>
      </v-card-title>
      <v-divider></v-divider>

      <v-data-table style="height: 440px;"
        :headers="headers"
        :items="servicios"
        :item-key="id">

        <template v-slot:header="{ props }">
          <tr>
            <th v-for="header in props.headers" :key="header.value" :style="{ backgroundColor: 'lightgray' }">
              {{ header.text }}
            </th>
          </tr>
        </template>
      </v-data-table>
  </v-card>
  </template>


  <script>
  import axios from 'axios';
  export default {
  data() {
    return {
      search: ' ',
      dialog: false,
      headers: [
        //Comenta lo de abajo   usa  para comentar /** */
        { title: 'Tipo', value: 'segmento'  },
        { title: 'Estado', value: 'status'  },
        { title: 'Consultor', value: 'consultor'  },
        { title: 'Proveedor', value: 'proveedor'  },
      ],
      servicios: []
    };
  },
  mounted() {
    this.fetchServicios();
  },
  methods: {
    async fetchServicios() {
      try {
        const response = await axios.get('http://localhost:3500/servicios');

        this.servicios = response.data;
      } catch (error) {
        console.error('Error fetching servicios:', error);
      }
    },
    async exportToExcel() {
      try {
        // Realizar solicitud GET al endpoint de exportación de Excel
        const response = await axios.get('http://localhost:3500/exportar-excel', {
          responseType: 'blob' // Establecer el tipo de respuesta como blob (archivo binario)
        });

        // Crear una URL a partir de los datos recibidos
        const url = window.URL.createObjectURL(new Blob([response.data]));

        // Crear un enlace y simular un clic para descargar el archivo
        const link = document.createElement('a');
        link.href = url;
        link.setAttribute('download', 'servicios.xlsx');
        document.body.appendChild(link);
        link.click();

        // Limpiar la URL creada
        window.URL.revokeObjectURL(url);
      } catch (error) {
        console.error('Error exporting to Excel:', error);
      }
    },
  }
  };
  </script>
  <style>
    .claseCabeceraTabla{
      text-align: center;

    }
    .card{
      padding: 0;
      border: 0;
      margin: 0;
    }
    .tituloNS{
      text-align: center;

      font-size: medium;
      font-family: Georgia, 'Times New Roman', Times, serif;
      color: #008080;
    }
  </style>
