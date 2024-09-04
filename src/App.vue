<template>
      <div class="floating-bar">
        
        <div @click="setu()" data-tooltip="Brush Tool"><i class="fa-solid fa-paint-brush"></i></div>
        <div @click="erase()" data-tooltip="Eraser"><i class="fa-solid fa-eraser"></i></div>
        <input v-model='colorNow' class ='coloresh' type="color" value="#000000" data-tooltip="Choose ur color!"> 
        <div @click="imager()" data-tooltip="Download image"><i class="fa-solid fa-download"></i></div>
        <div @click="saveJson()" data-tooltip="Save progress"><i class="fa-solid fa-floppy-disk"></i></div>
        <div @click="loadFile()" data-tooltip="Load Progress(In development)"><i class="fa-solid fa-file"></i></div>
        <div @click="reset()" data-tooltip="Clear Canvas"><i class="fa-solid fa-brush"></i></div>


      </div>
    <div class="mainContainer">
        <form @submit.prevent ="createArray">
          <input v-model='x' type="number" />
          <span style="color: white;">x</span>
          <input v-model="y" type="number">
        </form>
    </div>
    <div class="mainContainer" style="margin-top: 50px; width: 100%; max-width: 100vw; background-color: rgb(242, 247, 250);" > 
      <div v-for="j in x" >
          <div v-for="i in y" >
            <div class="indiv" @mouseover="returnid($event,j,i)" @mousedown="startDrawing()" @mouseup="stopDrawing()" style="width:25px; height:25px; background-color: rgba(224, 223, 223, 0); border:solid rgb(28, 26, 26) 0.8px;" :id="`${i}-${j}`">

            </div>
          </div>
      </div>
    </div>
    
    <canvas id="canva" style="background-color: azure; display: none;"></canvas>
</template>
<script>
  export default{
    methods:{
      // loadFile(){
      //   const fileInput = document.createElement('input');
      //   fileInput.type = 'file';
      //   fileInput.style.display = 'none';
      //   fileInput.accept = 'application/json';
      //   document.body.appendChild(fileInput);
      //   fileInput.click();

      //   fileInput.addEventListener('change',function(event){
      //     const file = event.target.files[0];
      //     if(file){
      //       const reader = new FileReader();
      //       reader.onload = function(e) {
      //           const content = JSON.parse(e.target.result)
      //           resolve(content)
      //           this.x = Object.keys(content).length
      //           this.y = content[0].length
      //           this.color = Array.from({ length: this.y }, () => 
      //             Array.from({ length: this.x }, () => 'rgba(0, 0, 0, 0)')
      //           );
      //           for(let i= 0;i<this.x;i++){
      //             for(let j=0;j<this.y;j++){
      //               this.color[i][j] = content[i][j]
      //             }
      //           }
      //           console.log(this.color)
      //       };
      //       reader.readAsText(file);
      //     }
      //     else{
      //       document.getElementById('fileContent').textContent = 'No file selected';
      //     }
      //     document.body.removeChild(fileInput);
      //   })
        
      // },
      saveJson(){
        const obj = { ...this.color };  // Using spread syntax
        const jsonFile = JSON.stringify(obj)
        const blob = new Blob([jsonFile], { type: "application/json" });
        const link = document.createElement("a");
        link.download = "arrayData.json";
        link.href = URL.createObjectURL(blob);
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
      },
      setu(){
        let color = document.querySelector('.coloresh')
        this.colorNow = color.value
      },
      erase(){
        this.colorNow ='rgba(0,0,0,0)'
      },
      reset(){
        location.reload();
      },
      imager(){
        const canvas = document.getElementById('canva')
        const context = canvas.getContext('2d');

        const pixelSize = 5; 
        canvas.width = this.x * pixelSize;
        canvas.height = this.y * pixelSize;

        for (let y = 0; y < this.y; y++) {
          for (let x = 0; x < this.x; x++) {
            
            context.fillStyle = this.color[y][x];
            
            context.fillRect(x * pixelSize,y * pixelSize, pixelSize, pixelSize);
            
            
          }
      }
      const dataURL = canvas.toDataURL('image/png');
      
      const link = document.createElement('a');
      link.href = dataURL;
      link.download = 'output.png'; 

      link.click();
      },
      startDrawing(){
        this.draw = true
      },
      stopDrawing(){
        this.draw = false
      },
      returnid(e,i,j){
        if(this.draw == false){
          return
        }

        let event = e.target.id
        let elem = document.getElementById(event)
        elem.style.backgroundColor = this.colorNow
        this.color[j-1][i-1] = this.colorNow
        
        
      }
    },
    data(){
      return{
        x:30,
        y:20,
        array: [],
        draw:false,
        colorNow:'#000000',
        color:[],
        loadfile:{}

      }
    },
    watch:{
      x(newValue, oldValue) {
        console.log(`x changed from ${oldValue} to ${newValue}`);
        this.color = []
        this.color = Array.from({ length: this.y }, () => 
          Array.from({ length: this.x }, () => 'rgba(0, 0, 0, 0)')
        );
       
      },
      y(newValue, oldValue) {
        console.log(`y changed from ${oldValue} to ${newValue}`);
        this.color = []
        this.color = Array.from({ length: this.y }, () => 
          Array.from({ length: this.x }, () => 'rgba(0, 0, 0, 0)')
        );
        console.log(this.color)
      }
    },
    created() {
      this.color = Array.from({ length: this.y }, () => 
        Array.from({ length: this.x }, () => 'rgba(0, 0, 0, 0)')
      );
      
    }
    }
  


</script>
<style>
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  .mainContainer{
    display: flex;
    justify-content: center;
    align-items: center;
  }
  input[type=number]{
    padding:8px;
    border-radius: 8px;
    margin-left: 5px;
    margin-right:5px;
  }
  .colorer{
    background-color: black ;
  }
  .floating-bar {
  
  position: fixed;  
  top: 150px;       
  left: 10px;  
  height: 470px;        
  width: 50px;     
  padding: 10px;
  background-color: #333;
  color: white;
  text-align: center;
  z-index: 1000;    
  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.3);
}
input[type=color]{
    box-sizing: unset;
    block-size: 25px;
    inline-size: 25px;
    border: none;
    padding: 0; /* Remove padding */
    width: 100%; /* Make the input fill the wrapper */
    appearance: none;
  }
.floating-bar >div{
  margin-top: 30px;
  margin-bottom: 30px;
}
.floating-bar >div::after{
  content: attr(data-tooltip);
  position: relative;
  top:0px;
  left: 50px;
  transform: translateX(-50%);
  padding: 5px;
  background-color: #333;
  color: #fff;
  border-radius: 3px;
  font-size: 12px;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  transition: opacity 0.3s, visibility 0.3s;
}
.floating-bar>div:hover::after{
  opacity: 1;
  visibility: visible;
}
.floating-bar >div > i{
  transition: 0.3s;
}
.floating-bar >div > i:active{
  color:rgb(9, 178, 216);
}
</style>