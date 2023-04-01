# Recovery of gold from ore

## Introduction and description

The project is provided by the [Zyfra](https://www.zyfra.com/) company. The company develops solutions for the efficient operation of industrial enterprises.    

**Task:**

The provided datasets contain parameters of gold flotation process. The data is indexed by the date and time the information was received  (`date` feature in datasets). Data is collected every hour. Parameters adjacent in time are often similar.  
  
Prepare a prototype machine learning model.   
The model should predict the recovery rate of gold from gold ore.  
The model will help optimize production so as not to launch an enterprise with unprofitable characteristics.

**Technological process:**

When the mined ore undergoes primary processing, a crushed mixture is obtained. It is sent for flotation (enrichment) and two-stage purification:

![pr.PNG](attachment:pr.PNG)

### Data naming description

Feature names look like this: `[stage].[parameter_type].[parameter_name]`  
Example: `rougher.input.feed_ag`  
  
***Possible values for block `[stage]`:***  
`rougher`: flotation stage  
`primary_cleaner`: primary cleaning stage  
`secondary_cleaner`: secondary cleaning stage  
`final`: final characteristics  
  
***Possible values for block `[parameter_type]`:***  
`input`: raw material parameters  
`output`: product parameters  
`state`: parameters characterizing stage  
`calculation`: calculated characteristics  
  
***Possible values for block `[parameter_name]`:***  
`feed_{component}`: feedstock component concentration, %  
`tail_{component}`: tail component concentration, %  
`concentrate_{component}`: component concentration, %  
`feed_rate`: input feed rate  
`feed_size`: input granule size  
`floatbank_..._air`: air volume at process stage  
`floatbank_..._level`: water level at process stage  
`floatbank_..._{reagent}`: amount of added flotation reagent at process stage 
  
Flotation reagents:
* Xanthate: collector reagent;  
* Sulphate (in this production, sodium sulfide);  
* Depressant (sodium silicate).  


### Calculations and task metrics

1. ***Recovery.*** Formula for recovery calculation:

![image.png](attachment:image.png)

where:  
***recovery*** is the recovery percentage(`rougher.output.recovery`);  
***C*** is the share of gold in the concentrate after flotation/cleaning (`rougher.output.concentrate_au`);  
***F*** is the share of gold in the raw material/concentrate before flotation/cleaning(`rougher.input.feed_au`);  
***T*** is the share of gold in final tailings after flotation/cleaning (`rougher.output.tail_au`).  

2. **SMAPE (Symmetric Mean Absolute Percentage Error)** is used as the main metric for models evaluation in this task:

![image.png](attachment:image.png)

where:  
***y<sub>i</sub>*** is the target recovery percentage;  
***ŷ<sub>i</sub>*** is the predicted recovery percentage;  
***N*** is the number of observations in the dataset.  

SMAPE is calculated separately for model predictions on `rougher.output.recovery` and `final.output.recovery`, and then two metrics are weighed to produce the final result:

![smape.png](attachment:smape.png)