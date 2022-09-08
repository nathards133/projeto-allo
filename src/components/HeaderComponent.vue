<template>
  <b-navbar  width="20%" toggleable="lg" type="dark" variant="primary">
    <b-container>
      <b-row class="text-white">
        <b-col>
          <div class="container">
          <!-- <img
          width="30%"
            alt="Allo Grupo"
            src="https://media-exp1.licdn.com/dms/image/C4D0BAQEmUOPIPcOB3A/company-logo_200_200/0/1601650462487?e=1670457600&v=beta&t=MX0ayaU1PT2rZv-980XBJ7bsQs_fG0X52zu0TxxOyC8
            "/> -->
          </div>
        </b-col>
          <span class="text-center h3">Plataforma de Cadastro de Clientes</span>
          <b-col cols="2">
            <b-row>
              <b-col>
                <b-button
                  variant="primary"
                  id="show-btn"
                  @click="$router.push('register')"
                >
                  <span class="h6 text-white">Novo Cliente</span>
                </b-button>
              </b-col>
            </b-row>
          </b-col>
      </b-row>
    </b-container>
  </b-navbar>
</template>
<script>
export default {
    name: "HeaderBar",
};
</script>
