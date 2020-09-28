<template>
  <div class="row-fluid">
    <div class="col-12">
      <div class="row-fluid">
        <form action="" id="formCalculateRoute">
          <div class="row">
            <div  class="col-4">
              <div class="form-group">
                <label for="">Subir</label>
                <input type="number" name="rise" id="rise" v-model="rise" class="form-control">
              </div>
            </div>
            <div  class="col-4">
              <div class="form-group">
                <label for="">Descer</label>
                <input type="number" name="fall" id="fall" v-model="fall" class="form-control">
              </div>
            </div>
            <div  class="col-4">
              <div class="form-group">
                <label for="">Altura total</label>
                <input type="number" name="height" id="height" v-model="height" class="form-control">
              </div>
            </div>
          </div>
          <div class="row">
            <div class="col-12">
              <button type="button" class="btn btn-dark btn-lg btn-block" @click="calculateRoute()">Calcular</button>
            </div>
          </div>        
        </form>
      </div>
      <div v-if="this.showResult" class="row">
        <div class="col-8 mt-4">
          <div id="square" :style="this.myStyle">            
          </div>
        </div>
        <div class="col-4 mt-4">
          <table class="table table-dark">
            <thead>
              <tr>
                <th colspan="2">Dados da rota</th>
              </tr>
              <tr>
                <th colspan="1">Subiu</th>
                <th colspan="1">Desceu</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>{{quantityRise}}</td>
                <td>{{quantityFall}}</td>
              </tr>

            </tbody>
          </table>
        </div>
      </div>
            
    </div>    
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    height: { type:Number, default: 20 },
    rise: { type:Number, default: 5 },
    fall: { type:Number, default: 3 },
    showResult: { type:Boolean, default: false },
    quantityFall: { type:Number, default: 0 },
    quantityRise: { type:Number, default: 0 },
    myStyle:{
      backgroundColor: "white"
    }
           
  },
  methods: {
    calculateRoute() {    

      console.log()
      if(this.height == 0 || this.rise == 0 || this.fall == 0) {
        alert("Todos parâmetros devem ser preenchidos.");
        return;
      }
      
      let data = {
        height: this.height,
        rise: this.rise,
        fall: this.fall
      }
      this.$http.post( 'http://localhost:3000/calculateroute', data)
      .then( response => {           
          console.log(response.body);
          this.quantityRise =  response.body.quantityRise;
          this.quantityFall = response.body.quantityFall;
          this.animation(0);       
      })
      .catch( () => {
          
      });
    },
    animation(count) {
      this.showResult = true;        

      count += this.rise - this.fall;            
      
      if( count >= this.height / 2)
        this.myStyle.backgroundColor = "yellow";
      
      if( count >= this.height)
        this.myStyle.backgroundColor = "green";
      
      console.log(count);
      if(count <= this.height) {
        
        //var  self = this;
        setTimeout( function () { this.animation(count) }.bind(this), 1000 );
      }
                   
    }
  },
  mounted(){

    this.myStyle = {
      backgroundColor: "white"
    }
  }
}
</script>

<style scoped>
#square{
  height:400px; 
  width:400px; 
  border-style:solid;
}
</style>
