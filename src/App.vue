<template>
  <div id="app">
    <el-tabs v-model="activeName">
      <el-tab-pane label="FIB" name="FIB">
        <el-row v-for="(item, index) in FIBList" :key="index" >
          <el-divider></el-divider>
          <el-col :span="6">
            <div class="input-q-div">{{item.topic}}</div>
            <div class="input-q-annotation-div">{{item.annotation}}</div>
          </el-col>
          <el-col :span="6">
            <div class="input-div">
              <el-input v-model="item.input" :placeholder="item.placeholder" onkeypress='return event.charCode >= 48 && event.charCode <= 57'>
                <template slot="append">{{item.unit}}</template>
              </el-input>
            </div>
          </el-col>
        </el-row>
        <div class="results-div" v-if="!FIBresults">
          Result:
          <div class="results-prompt-div">Please fill out required fields.</div>
        </div>
        <div class="results-div" v-else>
          {{FIBresults}} points
          <div class="results-prompt-div">Use alternative fibrosis assessment : {{FIBGrade}} </div>
        </div>
      </el-tab-pane>
      <el-tab-pane label="APRI" name="APRI">
        <el-row v-for="(item, index) in APRIList" :key="index" >
          <el-divider></el-divider>
          <el-col :span="6">
            <div class="input-q-div">{{item.topic}}</div>
            <div class="input-q-annotation-div">{{item.annotation}}</div>
          </el-col>
          <el-col :span="6">
            <div class="input-div">
              <el-input  v-model="item.input" :placeholder="item.placeholder" onkeypress='return event.charCode >= 48 && event.charCode <= 57'>
                <template slot="append">{{item.unit}}</template>
              </el-input>
            </div>
          </el-col>
        </el-row>
        <div class="results-div" v-if="!APRIresults">
          Result:
          <div class="results-prompt-div">Please fill out required fields.</div>
        </div>
        <div class="results-div" v-else>
          {{APRIresults}} points
          <div class="results-prompt-div">????</div>
        </div>
      </el-tab-pane>
      <el-tab-pane label="NAFLD" name="NAFLD">
        <el-row>
          <el-col :span="6" class="input-q-div">
            Impaired fasting glucose/diabetes
          </el-col>
          <el-col :span="6" class="input-div">
            <el-radio-group v-model="hyperglycemia"  text-color="#fff" fill="#1bb193">
              <el-radio-button :label="0">NO</el-radio-button>
              <el-radio-button :label="1">YES</el-radio-button>
            </el-radio-group>
          </el-col>
        </el-row>
        <el-row v-for="(item, index) in NAFLDList" :key="index" >
          <el-divider></el-divider>
          <el-col :span="6">
            <div class="input-q-div">{{item.topic}}</div>
            <div class="input-q-annotation-div">{{item.annotation}}</div>
          </el-col>
          <el-col :span="6">
            <div class="input-div">
              <el-input  v-model="item.input" :placeholder="item.placeholder" onkeypress='return event.charCode >= 48 && event.charCode <= 57'>
                <template slot="append">{{item.unit}}</template>
              </el-input>
            </div>
          </el-col>
        </el-row>
        <div class="results-div" v-if="!NAFLDresults">
          Result:
          <div class="results-prompt-div">Please fill out required fields.</div>
        </div>
        <div class="results-div" v-else>
          {{NAFLDresults}} points
          <div class="results-prompt-div">Correlated Fibrosis Severity : {{NAFLDGrade}}</div>
        </div>
      </el-tab-pane>
      <el-tab-pane label="ALBI" name="ALBI">
        <el-row v-for="(item, index) in ALBIList" :key="index" >
          <el-divider></el-divider>
          <el-col :span="6">
            <div class="input-q-div">{{item.topic}}</div>
            <div class="input-q-annotation-div">{{item.annotation}}</div>
          </el-col>
          <el-col :span="6" >
            <div class="input-div">
              <el-input  v-model="item.input" :placeholder="item.placeholder" onkeypress='return event.charCode >= 48 && event.charCode <= 57'>
                <template slot="append">{{item.unit}}</template>
              </el-input>
            </div>
          </el-col>
        </el-row>
        <div class="results-div" v-if="!ALBIresults">
          Result:
          <div class="results-prompt-div">Please fill out required fields.</div>
        </div>
        <div class="results-div" v-else>
          {{ALBIresults}} points
          <div class="results-prompt-div">Grade {{ALBIGrade}} </div>
        </div>
      </el-tab-pane>
      <el-tab-pane label="Reach-B" name="Reach-B">
        <el-row v-for="(item, index) in ReachBList" :key="index" >
          <el-divider></el-divider>
          <el-col :span="6">
            <div class="input-q-div">{{item.topic}}</div>
          </el-col>
          <el-col :span="12" class="input-div">
            <el-radio-group v-model="item.input" :placeholder="item.placeholder" text-color="#fff" fill="#1bb193">
              <el-radio-button :label="item.value[indexs]" v-for="(items, indexs) in item.annotation" :key="indexs" >
                {{items}}
                <span class="reachBList-value-span">
                  +{{item.value[indexs]}}
                </span>
              </el-radio-button>
            </el-radio-group>
          </el-col>
        </el-row>
        <div class="results-div" v-if="!ReachBIresults">
          Result:
          <div class="results-prompt-div">Please fill out required fields.</div>
        </div>
        <div class="results-div" v-else>
          {{ReachBIresults}} points
          <div class="results-prompt-div">????</div>
        </div>
      </el-tab-pane>
      <el-tab-pane label="MELD及MELD-Na" name="MELD">
        <el-row>
          <el-col :span="6" class="input-q-div">
            Dialysis at least twice in the past week
          </el-col>
          <el-col :span="6" class="input-div">
            <el-radio-group v-model="wholeFrequency"  text-color="#fff" fill="#1bb193">
              <el-radio-button :label="0">NO</el-radio-button>
              <el-radio-button :label="1">YES</el-radio-button>
            </el-radio-group>
          </el-col>
        </el-row>
        <el-row v-for="(item, index) in MELDList" :key="index" >
          <el-divider></el-divider>
          <el-col :span="6">
            <div class="input-q-div">{{item.topic}}</div>
            <div class="input-q-annotation-div">{{item.annotation}}</div>
          </el-col>
          <el-col :span="6">
            <div class="input-div">
              <el-input  v-model="item.input" :placeholder="item.placeholder">
                <template slot="append">{{item.unit}}</template>
              </el-input>
            </div>
          </el-col>
        </el-row>
        <div class="results-div" v-if="!MELDresults">
          Result:
          <div class="results-prompt-div">Please fill out required fields.</div>
        </div>
        <div class="results-div" v-else>
          {{MELDresults}} points
          <div class="results-prompt-div">????</div>
        </div>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
    
  },
  data:() => {
    return  {
      FIBList:[
        {topic:'Age',unit:'years',annotation:'Use with caution in patients <35 or >65 years old, as the score has been shown to be less reliable in these patients',input:''},
        {topic:'AST',unit:'U/L',annotation:'Aspartate aminotransferase',input:'',placeholder:'Norm: 15 - 41'},
        {topic:'Platelet count',unit:'× 10⁹/L',annotation:'',input:'',placeholder:'Norm: 150 - 35'},
        {topic:'ALT',unit:'IU / L',annotation:'Alanine aminotransferase',input:'',placeholder:'Norm: 1 - 35'}
      ],
      APRIList:[
        {topic:'AST',unit:'U / L',annotation:'',input:'',placeholder:'Norm: 15 - 41'}, //天門冬氨酸轉氨酶
        {topic:'AST upper limit of normal',unit:'U / L',annotation:'',input:'40',placeholder:'Norm: 15 - 41'},
        {topic:'Platelet count',unit:'× 10⁹/L',annotation:'',input:'',placeholder:'Norm: 150 - 35'} //血小板
      ],
      NAFLDList:[
        {topic:'Age',unit:'years',annotation:'',input:'',placeholder:''}, //years
        {topic:'BMI',unit:'kg/m²',annotation:'',input:'',placeholder:'Norm: 20 - 25'},
        {topic:'AST',unit:'IU/L',annotation:'',input:'',placeholder:'Norm: 15 - 41'}, //天門冬氨酸轉氨酶
        {topic:'ALT',unit:'IU/L',annotation:'',input:'',placeholder:'Norm: 1 - 35'}, //血清轉胺酶
        {topic:'Platelet Count',unit:'× 10⁹/L',annotation:'',input:'',placeholder:'Norm: 150 - 350'}, //血小板計數
        {topic:'albumin',unit:'g/dl',annotation:'',input:'',placeholder:'Norm: 3.5 - 5.5'} //白蛋白
      ],
      ALBIList:[
        {topic:'Bilirubin',unit:'mg / dl',annotation:'',input:'',placeholder:'Norm: 0.3 - 1.9'},//胆红素
        {topic:'Albumin',unit:'g / dl',annotation:'',input:'',placeholder:'Norm: 3.5 - 5.5'}//白蛋白
      ],
      ReachBList:[ 
        { topic:'Sex',
          annotation:['Female','Male'],
          value:[ 0 , 2 ],
          input:-1
        },
        { topic:'Age',
          annotation:['30–34','35–39','40–44','45–49','50–54','55–59','60–65'],
          value:[ 0 , 1 , 2 , 3 , 4 , 5 , 6 ],
          input:-1
        },
        { topic:'ALT',
          annotation:['<15','15–44','≥45'],
          value:[ 0 , 1 , 2 ],
          input:-1
        },
        { topic:'HBeAg',
          annotation:['Negative','Positive'],
          value:[ 0 , 2 ],
          input:-1
        },
        { topic:'Hepatitis B virus DNA level, copies/mL',
          annotation:['<300 (undetectable)','300–9,999','10,000–99,999','100,000–999,999','≥10⁶'],
          value:[ 0 , 0 , 3 , 5 , 4 ],
          input:-1
        },
      ],
      MELDList:[
        {topic:'Creatinine',unit:'mg / dl',annotation:'肌酸酐',input:''},
        {topic:'Bilirubin',unit:'mg / dl',annotation:'胆红素',input:''},
        {topic:'INR',unit:'',annotation:'國際標準化比值',input:''},
        {topic:'Sodium',unit:'mmol / L',annotation:'NA',input:''}
      ],
      hyperglycemia:-1,
      MELDresults:'',
      wholeFrequency:'',
      activeName: 'FIB',
    }
  },
  computed:{
    FIBresults(){
      let FIB = this.FIBList
      if( FIB[0].input!='' && FIB[1].input!='' && FIB[2].input!='' && FIB[3].input!=''){
        return this.formatFloat((FIB[0].input*FIB[1].input)/(FIB[2].input*Math.sqrt(FIB[3].input)),2)
      }else{
        return ''
      }
    },
    FIBGrade(){
      if(this.FIBresults <= 1.3){
        return 'Low'
      }else if( this.FIBresults <= 3.25 ){
        return 'Intermediate'
      }else{
        return 'High'
      }
    },
    APRIresults(){
      let APRI = this.APRIList
      if( APRI[0].input!='' && APRI[1].input!='' && APRI[2].input!=''){
        return this.formatFloat((APRI[0].input/APRI[1].input*100)/APRI[2].input,2)
      }else{
        return ''
      }
    },
    NAFLDresults(){
      let APRI = this.NAFLDList
      if( APRI[0].input!='' && APRI[1].input!='' && APRI[2].input!='' && APRI[3].input!='' && APRI[4].input!='' && APRI[5].input!='' && this.hyperglycemia != -1){
        return this.formatFloat((-1.675+(0.037*APRI[0].input)+(0.094*APRI[1].input)+(1.13*this.hyperglycemia)+(0.99*(APRI[2].input/APRI[3].input))-(0.013*APRI[4].input)-(0.66*APRI[5].input)),2)
      }else{
        return ''
      }
    },
    NAFLDGrade(){
      if(this.NAFLDresults <= -1.455){
        return 'Low'
      }else if( this.NAFLDresults <= 0.672 ){
        return 'Intermediate'
      }else{
        return 'High'
      }
    },
    ALBIresults(){
      let ALBI = this.ALBIList
      if(ALBI[0].input!='' && ALBI[1].input!=''){
        return this.formatFloat((( Math.log10( ALBI[0].input * 17.1 ) * 0.66 ) + ( ALBI[1].input * 10 * (-0.085) ) ),2)
      }else{
        return ''
      }
    },
    ALBIGrade(){
      if(this.ALBIresults <= -2.6){
        return 1
      }else if( this.ALBIresults <= -1.39 ){
        return 2
      }else{
        return 3
      }
    },
    ReachBIresults(){
      let ReachB = this.ReachBList
      let sum = 0
      ReachB.forEach(Element => {
        sum += Element.input
      })
      if( ReachB[0].input!=-1 && ReachB[1].input!=-1 && ReachB[2].input!=-1 && ReachB[3].input!=-1 && ReachB[4].input!=-1){
        return sum
      }else{
        return ''
      }
    }
  },
  methods: {
    MELDCalculation(){
      let MELD = this.MELDList
      //[0] C
      //[1] B
      //[2] I
      //[3] S
      console.log(MELD);
      let Na = MELD[3].input
      if(MELD[0].input < 1){ MELD[0].input = 1 } //1
      if(MELD[1].input < 1){ MELD[1].input = 1 } //1
      if(MELD[2].input < 1){ MELD[2].input = 1 } //1
      if(MELD[3].input<125){
        Na = 125
      }else if(MELD[3].input>137){
        Na = 137
      }
      let MELD_i = 0.378*MELD[1].input + 1.12*MELD[2].input + 0.957*MELD[0].input + 6.43
      this.MELDresults = MELD_i + (1.32*(137-Na)) - (0.033*MELD_i*(137-Na)) 
    },
    formatFloat(num, pos){
      //四捨五入
      var size = Math.pow(10, pos);
      return Math.round(num * size) / size;
    }
  },
}
</script>
<style scoped>
  .el-radio-button >>> .el-radio-button__inner{
    border-right: 1px solid silver;
    background-color: #f0f0f0;
    color: #595959;
    font-weight: 400;
    text-align: left;
    line-height: 1.3;
    padding: 11px 10px 9px;
    height: auto;
  }
  .el-radio-button >>> .el-radio-button__orig-radio:checked{
    font-weight: 600;
  }
  .el-row >>> .el-divider{
    margin : 12px 0;
  }
  .el-input >>> .el-input-group__append{
    background-color: #f0f0f0;
  }
  .input-div >>> .el-input{
    min-width: 100%;
  }
  .input-div >>> .el-input-group__append{
    font-family: Facit,Calibri,Helvetica,sans-serif!important;
  }
  .input-q-div{
    font-size: 14.5px;
    color: #595959;
    font-family: Facit,Calibri,Helvetica,sans-serif!important;
  }
  .input-q-annotation-div{
    color: #A9A9A9;
    font-size: 14px;
    float: left;
    font-family: Facit,Calibri,Helvetica,sans-serif!important;
  }
  .input-div{
    margin-left: 10px;
    font-family: Facit,Calibri,Helvetica,sans-serif!important;
  }
  .results-div{
    margin-top: 15px;
    background-color: #117d67;
    max-width: 940px;
    padding:5px 5px;
    font-size: 32px;
    color: #fff;
    font-family: Facit,Calibri,Helvetica,sans-serif!important;
    box-sizing: border-box;
    font-weight: 700;
  }
  .results-prompt-div{
    opacity: .8;
    font-size: 16px;
    font-weight: 600;
  }
  .reachBList-value-span{
    font-size: 14px;
    opacity: .7;
    padding-left: 10px;
  }
</style>
