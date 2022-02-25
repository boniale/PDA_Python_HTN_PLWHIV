Prevalence of Hypertension among Persons Living with HIV (PLWHIV) in Cape Town
==============================================================================

.. raw:: html

   <!-- ### Data Analysis Plan:
   1. Introduction
   2. Exploratory data analysis
   3. Prevalence of hypertension in PLWHIV on ART or not
   4. Factors associated with hypertension in PLWHIV on ART or not
   5. Conclusion 
   6. Recommendations -->

*Boni Maxime Ale*

*NYC Data Science Academy*

*21 February 2022*

Introduction
------------

Sub-Saharan Africa (SSA) is experiencing a surge in the burden of
cardiovascular disease (CVD) [1]. In 2019, more than 1 million deaths
were attributable to CVD in sub‑Saharan Africa (SSA) alone [2]. CVD is
set to overtake infectious diseases as the leading cause of mortality in
the region in the next decade [3]. Hypertension is a major modifiable
risk factor for cardiovascular diseases (CVD) globally. In low- and
middle-income settings, including sub-Saharan Africa (SSA), hypertension
prevalence has been increasing rapidly over the past several decades.
The World Health Organization (WHO) estimates that 46% of individuals
>25 years in SSA have hypertension [4-6]. In Cape town, there is high
burden of hypertension with a prevalence of 40% in urban areas [7].

The widespread use of antiretroviral therapy (ART) in SSA has resulted
in a near normal life expectancy among persons living with with HIV
(PLWHIV); overall approximately 76% of PLWHIV in SSA are virally
suppressed [7] This increased lifespan, however, may lead to an
increased risk of non-communicable diseases (NCD), including
hypertension, due to the HIV virus and ART toxicity [8–11]. Studies on
hypertension in PLWHIV have shown varied results, some showing higher
prevalence of hypertension while others showing no differences or lower
prevalence of hypertension among PLWHIV [12, 13]. That’s why we aim to
explore in to contribute to the body of knowledge on the prevalence of
hypertension in SSA especially in South Africa.

1. Objectives
-------------

-  To Describe the prevalence of hypertension among those visiting a
   mobile clinic offering integrated chronic disease screening in high
   HIV disease burden, limited-resource settings in the Cape Town, South
   Africa, between 2008 and 2016.

-  To Compare this prevalence among age groups, gender, body mass index
   categories, level of CD4 and treatment groups.

-  To Determine the concommitant effect of age and gender on the
   distribution of hypertension prevalence

-  To identify if there is an impact of body mass index classes and
   gender on the distribution of hypertension prevalence

2. Methods
----------

-  **Data Set Description**

year: Year of mobile clinic visit

sex: Gender 1 = Male 2 = Female

age: (years)

priorhivtest: (Patient had been tested for HIV 0 = No prior to mobile
clinic visit) 1 = Yes

| treatment: HIV treatment status of patient 1 = On ART prior to mobile
  clinic visit 2 = Not on ART 3 = treatment unknown 4 = not on ART -
  defaulted hivresult: Result of HIV test 0 = Negative 1 = Known
  positive 2 = New positive hivnew: Result of HIV test (excluding
  patients that tested positive prior to mobile clinic visit) 0 =
  Negative
| 1 = New positive

cd4_cat: CD4 count category 1 = <50 cells/μl 2 = 50-249 cells/μl 3 =
250-349 cells/μl 4 = 350-499 cells/μl 5 = 500+ cells/μl

bp_cat: Blood pressure category 1 = Normal (<120 systolic AND <80
diastolic) 2 = Optimal (120-129 systolic OR 80-84 diastolic) 3 = High
normal (130-139 systolic OR 85-89 diastolic) 4 = Grade 1 (140-159
systolic OR 90-99 diastolic) 5 = Grade 2 (160-179 systolic OR 100-109
diastolic) 6 = Grade 3 (180+ systolic OR 110+ diastolic) 7 = Isolated
systolic (140+ systolic AND <90 diastolic)

bmi_cat: Body mass index category 1 = Underweight (<18.5) 2 = Normal
weight (18.5-24.9) 3 = Overweight (25-29.9) 4 = Obese (30+)

| stisymptoms: Patient reported at least one of the following symptoms:
  abnormal discharge, genital sores, pair during urination (males),
  abdominal pain (females), pain during intercourse (females)):
| 0 = No 1 = Yes

tbsymptoms: Patient reported at least one of the following tuberculosis
symptoms: cough, weight loss, fever, night sweat, loss of appetite,
blood in sputum diabsymptoms: Patient reported at least one of the
following diabetes symptoms: frequent urination, unexplained weight loss
or gain, increased thirst, unexplained fatigue. 0 = No 1 = Yes

| `Here <https://zivahub.uct.ac.za/articles/dataset/A_retrospective_longitudinal_study_conducted_in_Cape_Town_of_a_mobile_clinic_screening_for_HIV_and_chronic_disease_/14034653/1>`__
  is the link to the dataset.
| - **Data Analysis Methodology**

Firstly, I will start my analysis by data recoding which would lead to
changing the modality of some variables of interest and also renaming
certaining modalities of variables from numbers to more informative
name.

Secondly, I will do the data cleaning by removing some missing data and
filtering only patient with positive HIV test.

Thirdly, I will calculate the prevalence of hypertension among HIV
patients and see its distribution in different age groups, gender
groups, body mass index categories, treatment groups and CD4 categories.
I would be curious to see if there would be an impact of age groups and
gender at the same time in this prevalence distribution and also the
concommitant effect of body mass index and gender on the distribution of
hypertension prevalence.

-  **Packages Used** For this analysis, we used numpy, pandas and plotly
   express

3. Data Analysis and Results
----------------------------

3.1. Exploratory Data Analysis
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: ipython3

    ## Import library 
    import numpy as np 
    import pandas as pd
    #import matplotlib.pyplot as plt
    #plt.style.use('ggplot')
    import seaborn as sns
    import plotly
    import plotly.express as px
    plotly.offline.init_notebook_mode(connected = True)



.. raw:: html

    <script type="text/javascript">
    window.PlotlyConfig = {MathJaxConfig: 'local'};
    if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
    if (typeof require !== 'undefined') {
    require.undef("plotly");
    requirejs.config({
        paths: {
            'plotly': ['https://cdn.plot.ly/plotly-2.9.0.min']
        }
    });
    require(['plotly'], function(Plotly) {
        window._Plotly = Plotly;
    });
    }
    </script>
    


.. code:: ipython3

    ## Loading data
    hiv = pd.read_csv("dataset_plos.csv", 
                     # index_col = "year", 
                      parse_dates = True)
    
    ## check the first 5 observations of the data set
    hiv.head(5)




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>year</th>
          <th>sex</th>
          <th>age</th>
          <th>priorhivtest</th>
          <th>treatment</th>
          <th>hivresult</th>
          <th>hivnew</th>
          <th>cd4_cat</th>
          <th>bp_cat</th>
          <th>bmi_cat</th>
          <th>stisymptoms</th>
          <th>tbsymptoms</th>
          <th>diabsymptoms</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>2008</td>
          <td>1.0</td>
          <td>49.0</td>
          <td>0.0</td>
          <td>NaN</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>NaN</td>
          <td>5.0</td>
          <td>3.0</td>
          <td>0</td>
          <td>0</td>
          <td>0</td>
        </tr>
        <tr>
          <th>1</th>
          <td>2008</td>
          <td>1.0</td>
          <td>20.0</td>
          <td>0.0</td>
          <td>NaN</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>NaN</td>
          <td>2.0</td>
          <td>2.0</td>
          <td>0</td>
          <td>0</td>
          <td>0</td>
        </tr>
        <tr>
          <th>2</th>
          <td>2008</td>
          <td>2.0</td>
          <td>29.0</td>
          <td>0.0</td>
          <td>NaN</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>NaN</td>
          <td>2.0</td>
          <td>4.0</td>
          <td>0</td>
          <td>0</td>
          <td>0</td>
        </tr>
        <tr>
          <th>3</th>
          <td>2008</td>
          <td>2.0</td>
          <td>38.0</td>
          <td>1.0</td>
          <td>NaN</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>NaN</td>
          <td>1.0</td>
          <td>4.0</td>
          <td>0</td>
          <td>0</td>
          <td>0</td>
        </tr>
        <tr>
          <th>4</th>
          <td>2008</td>
          <td>1.0</td>
          <td>32.0</td>
          <td>0.0</td>
          <td>NaN</td>
          <td>0.0</td>
          <td>0.0</td>
          <td>NaN</td>
          <td>5.0</td>
          <td>2.0</td>
          <td>0</td>
          <td>0</td>
          <td>0</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython3

    ## let's create a copy of hiv dataset for data wrangling
    hiv2 = hiv.copy()
    
    ## let's convert the 7 modalities of bp_cat into two (normal and high)
    conversion_dictionary = {1 : "Normal",
                            2: "Normal",
                            3: "Normal",
                            4 : "High",
                            5 : "High",
                            6 : "High",
                            7 : "Normal"
                            }
    
    hiv2['bp_cat2'] = hiv2['bp_cat'].replace(conversion_dictionary)
    
    ## Age groups
    #print(hiv2["age"].value_counts())
    bins = [12, 20, 30, 40, 50, 60, 70]
    labels = ["12-20", "20-30", "30-40", "40-50", "50-60", "60+"]
    hiv2["age_group"] = pd.cut(hiv2["age"], bins = bins, labels = labels, right = False)
    
    ## change bmi_cat data type 
    hiv2['bmi_cat2'] = hiv2['bmi_cat'].astype('category')
    ## rename bmi category
    hiv2['bmi_cat2'] = hiv2['bmi_cat2'].cat.rename_categories({1: "Underweight",
                                                              2:"Normal weight",
                                                              3:"Overweight",
                                                              4:"Obese"})
    
    ## let's convert the 4 modalities of treatment into two 
    conversion_dictionary_trt = {1 : "On ART",
                                 2 : "Not on ART",
                                 3 :  "Not on ART",
                                 4 : "Not on ART"}
    
    hiv2['trt'] = hiv2['treatment'].replace(conversion_dictionary_trt)
    
    ## Gender
    ## change sex data type 
    hiv2['sex'] = hiv2['sex'].astype('category')
    ## rename gender category
    hiv2['sex'] = hiv2['sex'].cat.rename_categories({2:"female",1: "male"})
    
    
    ## cd4 renaming 
    hiv2['cd4_cat2'] = hiv2['cd4_cat'].astype('category')
    hiv2['cd4_cat2'] = hiv2['cd4_cat2'].cat.rename_categories({
       1 : "<50 cells/μl",
       2 : "50-249 cells/μl",
       3 : "250-349 cells/μl",
       4 : "350-499 cells/μl",
       5 : "500+ cells/μ"
    })
    

.. code:: ipython3

    ## select those who are HIV positive only 
    hiv2 = hiv2.loc[hiv2["hivresult"]!= 0]
    
    ## data cleaning 
    hiv2 = hiv2.dropna(subset = ["bp_cat2", "sex", "bmi_cat"])
    
    
    
    hiv3 = hiv2.loc[hiv2["bp_cat2"]== "High"]

3.2. Prevalence of Hypertension
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: ipython3

    prev_total1 = hiv2["bp_cat2"].value_counts() 
    prev_total = hiv2["bp_cat2"].value_counts(normalize = True) 
    prev_total = pd.DataFrame(round( prev_total* 100, 2))
    prev_total.columns = ["Prevalence of Hypertension"]
    print(prev_total1)
    prev_total


.. parsed-literal::

    Normal    4820
    High      1497
    Name: bp_cat2, dtype: int64
    



.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>Prevalence of Hypertension</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>Normal</th>
          <td>76.3</td>
        </tr>
        <tr>
          <th>High</th>
          <td>23.7</td>
        </tr>
      </tbody>
    </table>
    </div>



-  There are 23.7 % of PLWHIV who has hypertension.

-  This is a high prevalence in this population but still lower than the
   prevalence in the general population in Cape Town which was around
   `38.9% (95% confidence interval (CI): 35.6–42.3) in
   2013 <https://doi.org/10.1371/journal.pone.0078567>`__.

.. code:: ipython3

    prev_year = hiv2.groupby("year")["bp_cat2"].value_counts(normalize = True)
    prev_year = round(prev_year*100, 2)
    prev_year_bp = hiv2.groupby("year")["bp_cat2"].value_counts(normalize = True)
    prev_year_bp = pd.DataFrame(round(prev_year_bp*100, 2))
    prev_year_bp.columns = ["Prevalence of Hypertension (%)"]
    prev_year_bp = prev_year_bp.reset_index()
    prev_year_bp = prev_year_bp.loc[prev_year_bp["bp_cat2"]=="High"]
    prev_year_bp = prev_year_bp.drop(['bp_cat2'], axis = 1)
    prev_year_bp




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>year</th>
          <th>Prevalence of Hypertension (%)</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>1</th>
          <td>2008</td>
          <td>22.49</td>
        </tr>
        <tr>
          <th>3</th>
          <td>2009</td>
          <td>18.41</td>
        </tr>
        <tr>
          <th>5</th>
          <td>2010</td>
          <td>23.44</td>
        </tr>
        <tr>
          <th>7</th>
          <td>2011</td>
          <td>31.52</td>
        </tr>
        <tr>
          <th>9</th>
          <td>2012</td>
          <td>24.69</td>
        </tr>
        <tr>
          <th>11</th>
          <td>2014</td>
          <td>25.69</td>
        </tr>
        <tr>
          <th>13</th>
          <td>2015</td>
          <td>18.61</td>
        </tr>
        <tr>
          <th>15</th>
          <td>2016</td>
          <td>21.64</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython3

    ## Let's see the prevalence of hypertension over the year
    fig_prev = px.bar(prev_year_bp,
                            x = "year",
                            y = "Prevalence of Hypertension (%)", 
                      #color = "year",
                barmode = "group",
               # facet_col = "year",
                template="simple_white",
                title = "Prevalence of hypertension over the year",
                       text_auto='.4s'
                           )
    fig_prev.update_yaxes(title_text = "Prevalence of Hypertension (%)")
    fig_prev.update_xaxes(title_text = "Year of the Survey")
    fig_prev.update_xaxes(categoryorder = 'array', categoryarray= ['2013','2009','2015','2008','2016', '2012', '2014', '2010', '2011'])
    fig_prev.update_traces(textfont_size = 12, textangle = 0, textposition = "outside", cliponaxis = False)
    fig_prev.update_yaxes(range= [0, 35])
    fig_prev.show()



.. raw:: html

    <div>                            <div id="e9668b36-8c07-4dc2-b2b7-2efa1d1b156c" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("e9668b36-8c07-4dc2-b2b7-2efa1d1b156c")) {                    Plotly.newPlot(                        "e9668b36-8c07-4dc2-b2b7-2efa1d1b156c",                        [{"alignmentgroup":"True","hovertemplate":"year=%{x}<br>Prevalence of Hypertension (%)=%{y}<extra></extra>","legendgroup":"","marker":{"color":"#1F77B4","pattern":{"shape":""}},"name":"","offsetgroup":"","orientation":"v","showlegend":false,"textposition":"outside","texttemplate":"%{y:.4s}","x":[2008,2009,2010,2011,2012,2014,2015,2016],"xaxis":"x","y":[22.49,18.41,23.44,31.52,24.69,25.69,18.61,21.64],"yaxis":"y","type":"bar","textfont":{"size":12},"cliponaxis":false,"textangle":0}],                        {"template":{"data":{"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"bar":[{"error_x":{"color":"rgb(36,36,36)"},"error_y":{"color":"rgb(36,36,36)"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"carpet":[{"aaxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"baxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"choropleth"}],"contourcarpet":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"contourcarpet"}],"contour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"contour"}],"heatmapgl":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmapgl"}],"heatmap":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmap"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2dcontour"}],"histogram2d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2d"}],"histogram":[{"marker":{"line":{"color":"white","width":0.6}},"type":"histogram"}],"mesh3d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattermapbox"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolargl"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolar"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"rgb(237,237,237)"},"line":{"color":"white"}},"header":{"fill":{"color":"rgb(217,217,217)"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"colorscale":{"diverging":[[0.0,"rgb(103,0,31)"],[0.1,"rgb(178,24,43)"],[0.2,"rgb(214,96,77)"],[0.3,"rgb(244,165,130)"],[0.4,"rgb(253,219,199)"],[0.5,"rgb(247,247,247)"],[0.6,"rgb(209,229,240)"],[0.7,"rgb(146,197,222)"],[0.8,"rgb(67,147,195)"],[0.9,"rgb(33,102,172)"],[1.0,"rgb(5,48,97)"]],"sequential":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"sequentialminus":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]]},"colorway":["#1F77B4","#FF7F0E","#2CA02C","#D62728","#9467BD","#8C564B","#E377C2","#7F7F7F","#BCBD22","#17BECF"],"font":{"color":"rgb(36,36,36)"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"white","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"angularaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","radialaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"zaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"}},"shapedefaults":{"fillcolor":"black","line":{"width":0},"opacity":0.3},"ternary":{"aaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"baxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","caxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Year of the Survey"},"categoryorder":"array","categoryarray":["2013","2009","2015","2008","2016","2012","2014","2010","2011"]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Prevalence of Hypertension (%)"},"range":[0,35]},"legend":{"tracegroupgap":0},"title":{"text":"Prevalence of hypertension over the year"},"barmode":"group"},                        {"responsive": true}                    ).then(function(){
    
    var gd = document.getElementById('e9668b36-8c07-4dc2-b2b7-2efa1d1b156c');
    var x = new MutationObserver(function (mutations, observer) {{
            var display = window.getComputedStyle(gd).display;
            if (!display || display === 'none') {{
                console.log([gd, 'removed!']);
                Plotly.purge(gd);
                observer.disconnect();
            }}
    }});
    
    // Listen for the removal of the full notebook cells
    var notebookContainer = gd.closest('#notebook-container');
    if (notebookContainer) {{
        x.observe(notebookContainer, {childList: true});
    }}
    
    // Listen for the clearing of the current output cell
    var outputEl = gd.closest('.output');
    if (outputEl) {{
        x.observe(outputEl, {childList: true});
    }}
    
                            })                };                });            </script>        </div>


-  The prevalence of hypertension is relatively high in 2011 followed by
   the year 2014.
-  Since this is just a screening in the clinic, it will be difficult to
   explain why more patients was diagnosed with hyptertension on those
   particular years. It may be due to more awarness campaign for
   example.

3.2.1. Prevalence of Hypertension by age group
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: ipython3

    ## Prevalence of HTN by age group
    prev_age = hiv2.groupby("age_group")["bp_cat2"].value_counts(normalize = True)
    prev_age = pd.DataFrame(round(prev_age*100, 2))
    
    prev_age.columns = ["Prevalence of Hypertension (%)"]
    prev_age = prev_age.reset_index()
    prev_age = prev_age.loc[prev_age["bp_cat2"] == "High"].drop(['bp_cat2'], axis = 1)
    prev_age




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>age_group</th>
          <th>Prevalence of Hypertension (%)</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>1</th>
          <td>12-20</td>
          <td>13.08</td>
        </tr>
        <tr>
          <th>3</th>
          <td>20-30</td>
          <td>14.90</td>
        </tr>
        <tr>
          <th>5</th>
          <td>30-40</td>
          <td>22.23</td>
        </tr>
        <tr>
          <th>7</th>
          <td>40-50</td>
          <td>32.62</td>
        </tr>
        <tr>
          <th>9</th>
          <td>50-60</td>
          <td>39.44</td>
        </tr>
        <tr>
          <th>11</th>
          <td>60+</td>
          <td>40.00</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython3

    fig_prev_age = px.bar(prev_age,
                            x = "age_group",
                            y = "Prevalence of Hypertension (%)", 
                      #color = "sex",
                barmode = "group",
               # facet_col = "year",
                template="simple_white",
                title = "Prevalence of hypertension by age group",
                       text_auto='.3s'
                           )
    fig_prev_age.update_yaxes(title_text = "Prevalence of Hypertension (%)")
    fig_prev_age.update_xaxes(title_text = "Age Group")
    #fig_prev.update_xaxes(categoryorder = 'array', categoryarray= ['2013','2009','2015','2008','2016', '2012', '2014', '2010', '2011'])
    fig_prev_age.update_traces(textfont_size = 12, textangle = 0, textposition = "outside", cliponaxis = False)
    fig_prev_age.update_traces(marker_color='green')
    fig_prev_age.update_yaxes(range= [0, 50])
    fig_prev_age.show()



.. raw:: html

    <div>                            <div id="3ebe23b1-7f3e-486e-98eb-f130ddd40253" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("3ebe23b1-7f3e-486e-98eb-f130ddd40253")) {                    Plotly.newPlot(                        "3ebe23b1-7f3e-486e-98eb-f130ddd40253",                        [{"alignmentgroup":"True","hovertemplate":"age_group=%{x}<br>Prevalence of Hypertension (%)=%{y}<extra></extra>","legendgroup":"","marker":{"color":"green","pattern":{"shape":""}},"name":"","offsetgroup":"","orientation":"v","showlegend":false,"textposition":"outside","texttemplate":"%{y:.3s}","x":["12-20","20-30","30-40","40-50","50-60","60+"],"xaxis":"x","y":[13.08,14.9,22.23,32.62,39.44,40.0],"yaxis":"y","type":"bar","textfont":{"size":12},"cliponaxis":false,"textangle":0}],                        {"template":{"data":{"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"bar":[{"error_x":{"color":"rgb(36,36,36)"},"error_y":{"color":"rgb(36,36,36)"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"carpet":[{"aaxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"baxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"choropleth"}],"contourcarpet":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"contourcarpet"}],"contour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"contour"}],"heatmapgl":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmapgl"}],"heatmap":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmap"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2dcontour"}],"histogram2d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2d"}],"histogram":[{"marker":{"line":{"color":"white","width":0.6}},"type":"histogram"}],"mesh3d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattermapbox"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolargl"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolar"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"rgb(237,237,237)"},"line":{"color":"white"}},"header":{"fill":{"color":"rgb(217,217,217)"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"colorscale":{"diverging":[[0.0,"rgb(103,0,31)"],[0.1,"rgb(178,24,43)"],[0.2,"rgb(214,96,77)"],[0.3,"rgb(244,165,130)"],[0.4,"rgb(253,219,199)"],[0.5,"rgb(247,247,247)"],[0.6,"rgb(209,229,240)"],[0.7,"rgb(146,197,222)"],[0.8,"rgb(67,147,195)"],[0.9,"rgb(33,102,172)"],[1.0,"rgb(5,48,97)"]],"sequential":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"sequentialminus":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]]},"colorway":["#1F77B4","#FF7F0E","#2CA02C","#D62728","#9467BD","#8C564B","#E377C2","#7F7F7F","#BCBD22","#17BECF"],"font":{"color":"rgb(36,36,36)"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"white","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"angularaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","radialaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"zaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"}},"shapedefaults":{"fillcolor":"black","line":{"width":0},"opacity":0.3},"ternary":{"aaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"baxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","caxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Age Group"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Prevalence of Hypertension (%)"},"range":[0,50]},"legend":{"tracegroupgap":0},"title":{"text":"Prevalence of hypertension by age group"},"barmode":"group"},                        {"responsive": true}                    ).then(function(){
    
    var gd = document.getElementById('3ebe23b1-7f3e-486e-98eb-f130ddd40253');
    var x = new MutationObserver(function (mutations, observer) {{
            var display = window.getComputedStyle(gd).display;
            if (!display || display === 'none') {{
                console.log([gd, 'removed!']);
                Plotly.purge(gd);
                observer.disconnect();
            }}
    }});
    
    // Listen for the removal of the full notebook cells
    var notebookContainer = gd.closest('#notebook-container');
    if (notebookContainer) {{
        x.observe(notebookContainer, {childList: true});
    }}
    
    // Listen for the clearing of the current output cell
    var outputEl = gd.closest('.output');
    if (outputEl) {{
        x.observe(outputEl, {childList: true});
    }}
    
                            })                };                });            </script>        </div>


-  We can clearly see with the above graph that the prevalence of
   hypertension increase as the patients get older. There is a clear
   relationship between age and the presence of hypertension or not.

-  This is similar to what is happen in the general population. As
   PLWHIV live longer, they are obviously exposed to the same CVD risk
   factors as the general population.

3.2.2. Prevalence of Hypertension by gender
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: ipython3

    ## Prevalence of HTN by gender
    prev_gender = hiv2.groupby("sex")["bp_cat2"].value_counts(normalize = True)
    prev_gender= pd.DataFrame(round(prev_gender*100, 2))
    prev_gender.columns = ["Prevalence of Hypertension"]
    prev_gender= prev_gender.reset_index()
    prev_gender = prev_gender.loc[prev_gender["bp_cat2"] == "High"].drop(['bp_cat2'], axis = 1)
    prev_gender




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>sex</th>
          <th>Prevalence of Hypertension</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>1</th>
          <td>male</td>
          <td>23.79</td>
        </tr>
        <tr>
          <th>3</th>
          <td>female</td>
          <td>23.63</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython3

    fig_prev_gender = px.bar(prev_gender,
                            x = "sex",
                            y = "Prevalence of Hypertension", 
                     # color = "sex",
                barmode = "group",
               # facet_col = "year",
                template="simple_white",
                title = "Prevalence of hypertension among men and women in PLWHIV",
                       text_auto='.3s'
                           )
    fig_prev_gender.update_yaxes(title_text = "Prevalence of Hypertension")
    fig_prev_gender.update_xaxes(title_text = "Gender")
    #fig_prev.update_xaxes(categoryorder = 'array', categoryarray= ['2013','2009','2015','2008','2016', '2012', '2014', '2010', '2011'])
    fig_prev_gender.update_traces(textfont_size = 12, textangle = 0, textposition = "outside", cliponaxis = False)
    fig_prev_gender.update_yaxes(range= [0, 30])
    fig_prev_gender.show()



.. raw:: html

    <div>                            <div id="c528f2a0-a84d-4e7a-9be7-3ad3625ce70a" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("c528f2a0-a84d-4e7a-9be7-3ad3625ce70a")) {                    Plotly.newPlot(                        "c528f2a0-a84d-4e7a-9be7-3ad3625ce70a",                        [{"alignmentgroup":"True","hovertemplate":"sex=%{x}<br>Prevalence of Hypertension=%{y}<extra></extra>","legendgroup":"","marker":{"color":"#1F77B4","pattern":{"shape":""}},"name":"","offsetgroup":"","orientation":"v","showlegend":false,"textposition":"outside","texttemplate":"%{y:.3s}","x":["male","female"],"xaxis":"x","y":[23.79,23.63],"yaxis":"y","type":"bar","textfont":{"size":12},"cliponaxis":false,"textangle":0}],                        {"template":{"data":{"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"bar":[{"error_x":{"color":"rgb(36,36,36)"},"error_y":{"color":"rgb(36,36,36)"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"carpet":[{"aaxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"baxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"choropleth"}],"contourcarpet":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"contourcarpet"}],"contour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"contour"}],"heatmapgl":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmapgl"}],"heatmap":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmap"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2dcontour"}],"histogram2d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2d"}],"histogram":[{"marker":{"line":{"color":"white","width":0.6}},"type":"histogram"}],"mesh3d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattermapbox"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolargl"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolar"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"rgb(237,237,237)"},"line":{"color":"white"}},"header":{"fill":{"color":"rgb(217,217,217)"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"colorscale":{"diverging":[[0.0,"rgb(103,0,31)"],[0.1,"rgb(178,24,43)"],[0.2,"rgb(214,96,77)"],[0.3,"rgb(244,165,130)"],[0.4,"rgb(253,219,199)"],[0.5,"rgb(247,247,247)"],[0.6,"rgb(209,229,240)"],[0.7,"rgb(146,197,222)"],[0.8,"rgb(67,147,195)"],[0.9,"rgb(33,102,172)"],[1.0,"rgb(5,48,97)"]],"sequential":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"sequentialminus":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]]},"colorway":["#1F77B4","#FF7F0E","#2CA02C","#D62728","#9467BD","#8C564B","#E377C2","#7F7F7F","#BCBD22","#17BECF"],"font":{"color":"rgb(36,36,36)"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"white","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"angularaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","radialaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"zaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"}},"shapedefaults":{"fillcolor":"black","line":{"width":0},"opacity":0.3},"ternary":{"aaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"baxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","caxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Gender"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Prevalence of Hypertension"},"range":[0,30]},"legend":{"tracegroupgap":0},"title":{"text":"Prevalence of hypertension among men and women in PLWHIV"},"barmode":"group"},                        {"responsive": true}                    ).then(function(){
    
    var gd = document.getElementById('c528f2a0-a84d-4e7a-9be7-3ad3625ce70a');
    var x = new MutationObserver(function (mutations, observer) {{
            var display = window.getComputedStyle(gd).display;
            if (!display || display === 'none') {{
                console.log([gd, 'removed!']);
                Plotly.purge(gd);
                observer.disconnect();
            }}
    }});
    
    // Listen for the removal of the full notebook cells
    var notebookContainer = gd.closest('#notebook-container');
    if (notebookContainer) {{
        x.observe(notebookContainer, {childList: true});
    }}
    
    // Listen for the clearing of the current output cell
    var outputEl = gd.closest('.output');
    if (outputEl) {{
        x.observe(outputEl, {childList: true});
    }}
    
                            })                };                });            </script>        </div>


The prevalence of hypertension is similar among women and men among this
population of PLWHIV.

3.2.3. Prevalence of hypertension by age group and gender
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Is there any particular trend in the distribution of the hypertension
prevalence with age and gender ?

.. code:: ipython3

    prev_age_sex = hiv2.groupby(["age_group","sex"])["bp_cat2"].value_counts(normalize = True)
    prev_age_sex = pd.DataFrame(round(prev_age_sex*100, 2))
    prev_age_sex.columns = ["Prevalence of Hypertension"]
    prev_age_sex = prev_age_sex.reset_index()
    prev_age_sex = prev_age_sex.loc[prev_age_sex["bp_cat2"]== "High"].drop(['bp_cat2'], axis = 1)
    prev_age_sex 




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>age_group</th>
          <th>sex</th>
          <th>Prevalence of Hypertension</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>1</th>
          <td>12-20</td>
          <td>male</td>
          <td>16.28</td>
        </tr>
        <tr>
          <th>3</th>
          <td>12-20</td>
          <td>female</td>
          <td>12.37</td>
        </tr>
        <tr>
          <th>5</th>
          <td>20-30</td>
          <td>male</td>
          <td>13.42</td>
        </tr>
        <tr>
          <th>7</th>
          <td>20-30</td>
          <td>female</td>
          <td>15.51</td>
        </tr>
        <tr>
          <th>9</th>
          <td>30-40</td>
          <td>male</td>
          <td>23.34</td>
        </tr>
        <tr>
          <th>11</th>
          <td>30-40</td>
          <td>female</td>
          <td>21.20</td>
        </tr>
        <tr>
          <th>13</th>
          <td>40-50</td>
          <td>male</td>
          <td>28.20</td>
        </tr>
        <tr>
          <th>15</th>
          <td>40-50</td>
          <td>female</td>
          <td>37.13</td>
        </tr>
        <tr>
          <th>17</th>
          <td>50-60</td>
          <td>male</td>
          <td>36.04</td>
        </tr>
        <tr>
          <th>19</th>
          <td>50-60</td>
          <td>female</td>
          <td>41.95</td>
        </tr>
        <tr>
          <th>21</th>
          <td>60+</td>
          <td>male</td>
          <td>43.75</td>
        </tr>
        <tr>
          <th>23</th>
          <td>60+</td>
          <td>female</td>
          <td>38.46</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython3

    fig_prev_age_sex = px.bar(prev_age_sex,
                            x = "age_group",
                            y = "Prevalence of Hypertension", 
                      color = "sex",
                barmode = "group",
               # facet_col = "year",
                template="simple_white",
                title = "Prevalence of hypertension by age group and gender",
                       text_auto='.3s'
                           )
    fig_prev_age_sex.update_yaxes(title_text = "Prevalence of Hypertension")
    fig_prev_age_sex.update_xaxes(title_text = "Age Group")
    #fig_prev.update_xaxes(categoryorder = 'array', categoryarray= ['2013','2009','2015','2008','2016', '2012', '2014', '2010', '2011'])
    fig_prev_age_sex.update_traces(textfont_size = 12, textangle = 0, textposition = "outside", cliponaxis = False)
    fig_prev_age_sex.update_yaxes(range= [0, 50])
    fig_prev_age_sex.show()



.. raw:: html

    <div>                            <div id="934ea59c-a965-4feb-ab71-0a16a64cc5af" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("934ea59c-a965-4feb-ab71-0a16a64cc5af")) {                    Plotly.newPlot(                        "934ea59c-a965-4feb-ab71-0a16a64cc5af",                        [{"alignmentgroup":"True","hovertemplate":"sex=male<br>age_group=%{x}<br>Prevalence of Hypertension=%{y}<extra></extra>","legendgroup":"male","marker":{"color":"#1F77B4","pattern":{"shape":""}},"name":"male","offsetgroup":"male","orientation":"v","showlegend":true,"textposition":"outside","texttemplate":"%{y:.3s}","x":["12-20","20-30","30-40","40-50","50-60","60+"],"xaxis":"x","y":[16.28,13.42,23.34,28.2,36.04,43.75],"yaxis":"y","type":"bar","textfont":{"size":12},"cliponaxis":false,"textangle":0},{"alignmentgroup":"True","hovertemplate":"sex=female<br>age_group=%{x}<br>Prevalence of Hypertension=%{y}<extra></extra>","legendgroup":"female","marker":{"color":"#FF7F0E","pattern":{"shape":""}},"name":"female","offsetgroup":"female","orientation":"v","showlegend":true,"textposition":"outside","texttemplate":"%{y:.3s}","x":["12-20","20-30","30-40","40-50","50-60","60+"],"xaxis":"x","y":[12.37,15.51,21.2,37.13,41.95,38.46],"yaxis":"y","type":"bar","textfont":{"size":12},"cliponaxis":false,"textangle":0}],                        {"template":{"data":{"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"bar":[{"error_x":{"color":"rgb(36,36,36)"},"error_y":{"color":"rgb(36,36,36)"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"carpet":[{"aaxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"baxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"choropleth"}],"contourcarpet":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"contourcarpet"}],"contour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"contour"}],"heatmapgl":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmapgl"}],"heatmap":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmap"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2dcontour"}],"histogram2d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2d"}],"histogram":[{"marker":{"line":{"color":"white","width":0.6}},"type":"histogram"}],"mesh3d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattermapbox"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolargl"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolar"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"rgb(237,237,237)"},"line":{"color":"white"}},"header":{"fill":{"color":"rgb(217,217,217)"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"colorscale":{"diverging":[[0.0,"rgb(103,0,31)"],[0.1,"rgb(178,24,43)"],[0.2,"rgb(214,96,77)"],[0.3,"rgb(244,165,130)"],[0.4,"rgb(253,219,199)"],[0.5,"rgb(247,247,247)"],[0.6,"rgb(209,229,240)"],[0.7,"rgb(146,197,222)"],[0.8,"rgb(67,147,195)"],[0.9,"rgb(33,102,172)"],[1.0,"rgb(5,48,97)"]],"sequential":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"sequentialminus":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]]},"colorway":["#1F77B4","#FF7F0E","#2CA02C","#D62728","#9467BD","#8C564B","#E377C2","#7F7F7F","#BCBD22","#17BECF"],"font":{"color":"rgb(36,36,36)"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"white","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"angularaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","radialaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"zaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"}},"shapedefaults":{"fillcolor":"black","line":{"width":0},"opacity":0.3},"ternary":{"aaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"baxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","caxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Age Group"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Prevalence of Hypertension"},"range":[0,50]},"legend":{"title":{"text":"sex"},"tracegroupgap":0},"title":{"text":"Prevalence of hypertension by age group and gender"},"barmode":"group"},                        {"responsive": true}                    ).then(function(){
    
    var gd = document.getElementById('934ea59c-a965-4feb-ab71-0a16a64cc5af');
    var x = new MutationObserver(function (mutations, observer) {{
            var display = window.getComputedStyle(gd).display;
            if (!display || display === 'none') {{
                console.log([gd, 'removed!']);
                Plotly.purge(gd);
                observer.disconnect();
            }}
    }});
    
    // Listen for the removal of the full notebook cells
    var notebookContainer = gd.closest('#notebook-container');
    if (notebookContainer) {{
        x.observe(notebookContainer, {childList: true});
    }}
    
    // Listen for the clearing of the current output cell
    var outputEl = gd.closest('.output');
    if (outputEl) {{
        x.observe(outputEl, {childList: true});
    }}
    
                            })                };                });            </script>        </div>


-  This means that there is almost no difference between the prevalence
   of hypertension in age groups among male and female.

-  The distribution of hypertension increased with people getting older
   and this does not depend on their gender.

3.2.4. Prevalence of Hypertension by CD4 count category
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: ipython3

    ## Prevalence of HTN by CD4 count category
    prev_cd4 = hiv2.groupby("cd4_cat2")["bp_cat2"].value_counts(normalize = True)
    prev_cd4 = pd.DataFrame(round(prev_cd4*100, 2))
    prev_cd4.columns = ["Prevalence of Hypertension"]
    prev_cd4 = prev_cd4.reset_index()
    prev_cd4 = prev_cd4.loc[prev_cd4["bp_cat2"] == "High"].drop(['bp_cat2'], axis = 1)
    prev_cd4




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>cd4_cat2</th>
          <th>Prevalence of Hypertension</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>1</th>
          <td>&lt;50 cells/μl</td>
          <td>21.21</td>
        </tr>
        <tr>
          <th>3</th>
          <td>50-249 cells/μl</td>
          <td>24.53</td>
        </tr>
        <tr>
          <th>5</th>
          <td>250-349 cells/μl</td>
          <td>23.86</td>
        </tr>
        <tr>
          <th>7</th>
          <td>350-499 cells/μl</td>
          <td>23.50</td>
        </tr>
        <tr>
          <th>9</th>
          <td>500+ cells/μ</td>
          <td>24.53</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython3

    fig_prev_cd4 = px.bar(prev_cd4,
                            x = "cd4_cat2",
                            y = "Prevalence of Hypertension", 
                      # color = "sex",
                barmode = "group",
               # facet_col = "year",
                template="simple_white",
                title = "Prevalence of hypertension by CD4 count category",
                       text_auto='.4s'
                           )
    fig_prev_cd4.update_yaxes(title_text = "Prevalence of Hypertension")
    fig_prev_cd4.update_xaxes(title_text = "CD4 count category")
    fig_prev_cd4.update_traces(textfont_size = 12, textangle = 0, textposition = "outside", cliponaxis = False)
    fig_prev_cd4.update_traces(marker_color = 'indianred')
    fig_prev_cd4.update_yaxes(range= [0, 35])
    fig_prev_cd4.show()



.. raw:: html

    <div>                            <div id="e5d1d122-c7d4-4a23-88a7-19965a0d51d5" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("e5d1d122-c7d4-4a23-88a7-19965a0d51d5")) {                    Plotly.newPlot(                        "e5d1d122-c7d4-4a23-88a7-19965a0d51d5",                        [{"alignmentgroup":"True","hovertemplate":"cd4_cat2=%{x}<br>Prevalence of Hypertension=%{y}<extra></extra>","legendgroup":"","marker":{"color":"indianred","pattern":{"shape":""}},"name":"","offsetgroup":"","orientation":"v","showlegend":false,"textposition":"outside","texttemplate":"%{y:.4s}","x":["<50 cells/\u03bcl","50-249 cells/\u03bcl","250-349 cells/\u03bcl","350-499 cells/\u03bcl","500+ cells/\u03bc"],"xaxis":"x","y":[21.21,24.53,23.86,23.5,24.53],"yaxis":"y","type":"bar","textfont":{"size":12},"cliponaxis":false,"textangle":0}],                        {"template":{"data":{"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"bar":[{"error_x":{"color":"rgb(36,36,36)"},"error_y":{"color":"rgb(36,36,36)"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"carpet":[{"aaxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"baxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"choropleth"}],"contourcarpet":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"contourcarpet"}],"contour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"contour"}],"heatmapgl":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmapgl"}],"heatmap":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmap"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2dcontour"}],"histogram2d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2d"}],"histogram":[{"marker":{"line":{"color":"white","width":0.6}},"type":"histogram"}],"mesh3d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattermapbox"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolargl"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolar"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"rgb(237,237,237)"},"line":{"color":"white"}},"header":{"fill":{"color":"rgb(217,217,217)"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"colorscale":{"diverging":[[0.0,"rgb(103,0,31)"],[0.1,"rgb(178,24,43)"],[0.2,"rgb(214,96,77)"],[0.3,"rgb(244,165,130)"],[0.4,"rgb(253,219,199)"],[0.5,"rgb(247,247,247)"],[0.6,"rgb(209,229,240)"],[0.7,"rgb(146,197,222)"],[0.8,"rgb(67,147,195)"],[0.9,"rgb(33,102,172)"],[1.0,"rgb(5,48,97)"]],"sequential":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"sequentialminus":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]]},"colorway":["#1F77B4","#FF7F0E","#2CA02C","#D62728","#9467BD","#8C564B","#E377C2","#7F7F7F","#BCBD22","#17BECF"],"font":{"color":"rgb(36,36,36)"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"white","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"angularaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","radialaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"zaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"}},"shapedefaults":{"fillcolor":"black","line":{"width":0},"opacity":0.3},"ternary":{"aaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"baxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","caxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"CD4 count category"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Prevalence of Hypertension"},"range":[0,35]},"legend":{"tracegroupgap":0},"title":{"text":"Prevalence of hypertension by CD4 count category"},"barmode":"group"},                        {"responsive": true}                    ).then(function(){
    
    var gd = document.getElementById('e5d1d122-c7d4-4a23-88a7-19965a0d51d5');
    var x = new MutationObserver(function (mutations, observer) {{
            var display = window.getComputedStyle(gd).display;
            if (!display || display === 'none') {{
                console.log([gd, 'removed!']);
                Plotly.purge(gd);
                observer.disconnect();
            }}
    }});
    
    // Listen for the removal of the full notebook cells
    var notebookContainer = gd.closest('#notebook-container');
    if (notebookContainer) {{
        x.observe(notebookContainer, {childList: true});
    }}
    
    // Listen for the clearing of the current output cell
    var outputEl = gd.closest('.output');
    if (outputEl) {{
        x.observe(outputEl, {childList: true});
    }}
    
                            })                };                });            </script>        </div>


-  PLWHIV with CD4 at 500+ cells/u are patients with controlled HIV.
-  There do not seems to be any difference in the prevalence of
   hypertension either your HIV is controlled or not not under
   treatment.

3.2.5. Prevalence of hypertension by Body Mass Index (BMI) category
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: ipython3

    ## let's see the proportion of hypertension by Body mass index category
    prev_bmi = hiv2.groupby("bmi_cat2")["bp_cat2"].value_counts(normalize = True)
    prev_bmi = pd.DataFrame(round(prev_bmi*100, 2))
    prev_bmi.columns = ["Prevalence of Hypertension"]
    prev_bmi = prev_bmi.reset_index()
    prev_bmi = prev_bmi.loc[prev_bmi["bp_cat2"] == "High"].drop(['bp_cat2'], axis = 1)
    prev_bmi




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>bmi_cat2</th>
          <th>Prevalence of Hypertension</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>1</th>
          <td>Underweight</td>
          <td>13.85</td>
        </tr>
        <tr>
          <th>3</th>
          <td>Normal weight</td>
          <td>18.95</td>
        </tr>
        <tr>
          <th>5</th>
          <td>Overweight</td>
          <td>24.92</td>
        </tr>
        <tr>
          <th>7</th>
          <td>Obese</td>
          <td>31.85</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython3

    fig_prev_bmi = px.bar(prev_bmi,
                            x = "bmi_cat2",
                            y = "Prevalence of Hypertension", 
                      # color = "sex",
                barmode = "group",
               # facet_col = "year",
                template="simple_white",
                title = "Prevalence of hypertension by body mass index category",
                       text_auto='.4s'
                           )
    fig_prev_bmi.update_yaxes(title_text = "Prevalence of Hypertension")
    fig_prev_bmi.update_xaxes(title_text = "Body mass index category")
    fig_prev_bmi.update_traces(textfont_size = 12, textangle = 0, textposition = "outside", cliponaxis = False)
    fig_prev_bmi.update_traces(marker_color = 'red')
    fig_prev_bmi.update_yaxes(range= [0, 40])
    fig_prev_bmi.show()



.. raw:: html

    <div>                            <div id="c5454eb4-9608-4b7f-be62-b4ea06a87966" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("c5454eb4-9608-4b7f-be62-b4ea06a87966")) {                    Plotly.newPlot(                        "c5454eb4-9608-4b7f-be62-b4ea06a87966",                        [{"alignmentgroup":"True","hovertemplate":"bmi_cat2=%{x}<br>Prevalence of Hypertension=%{y}<extra></extra>","legendgroup":"","marker":{"color":"red","pattern":{"shape":""}},"name":"","offsetgroup":"","orientation":"v","showlegend":false,"textposition":"outside","texttemplate":"%{y:.4s}","x":["Underweight","Normal weight","Overweight","Obese"],"xaxis":"x","y":[13.85,18.95,24.92,31.85],"yaxis":"y","type":"bar","textfont":{"size":12},"cliponaxis":false,"textangle":0}],                        {"template":{"data":{"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"bar":[{"error_x":{"color":"rgb(36,36,36)"},"error_y":{"color":"rgb(36,36,36)"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"carpet":[{"aaxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"baxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"choropleth"}],"contourcarpet":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"contourcarpet"}],"contour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"contour"}],"heatmapgl":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmapgl"}],"heatmap":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmap"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2dcontour"}],"histogram2d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2d"}],"histogram":[{"marker":{"line":{"color":"white","width":0.6}},"type":"histogram"}],"mesh3d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattermapbox"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolargl"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolar"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"rgb(237,237,237)"},"line":{"color":"white"}},"header":{"fill":{"color":"rgb(217,217,217)"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"colorscale":{"diverging":[[0.0,"rgb(103,0,31)"],[0.1,"rgb(178,24,43)"],[0.2,"rgb(214,96,77)"],[0.3,"rgb(244,165,130)"],[0.4,"rgb(253,219,199)"],[0.5,"rgb(247,247,247)"],[0.6,"rgb(209,229,240)"],[0.7,"rgb(146,197,222)"],[0.8,"rgb(67,147,195)"],[0.9,"rgb(33,102,172)"],[1.0,"rgb(5,48,97)"]],"sequential":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"sequentialminus":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]]},"colorway":["#1F77B4","#FF7F0E","#2CA02C","#D62728","#9467BD","#8C564B","#E377C2","#7F7F7F","#BCBD22","#17BECF"],"font":{"color":"rgb(36,36,36)"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"white","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"angularaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","radialaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"zaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"}},"shapedefaults":{"fillcolor":"black","line":{"width":0},"opacity":0.3},"ternary":{"aaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"baxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","caxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Body mass index category"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Prevalence of Hypertension"},"range":[0,40]},"legend":{"tracegroupgap":0},"title":{"text":"Prevalence of hypertension by body mass index category"},"barmode":"group"},                        {"responsive": true}                    ).then(function(){
    
    var gd = document.getElementById('c5454eb4-9608-4b7f-be62-b4ea06a87966');
    var x = new MutationObserver(function (mutations, observer) {{
            var display = window.getComputedStyle(gd).display;
            if (!display || display === 'none') {{
                console.log([gd, 'removed!']);
                Plotly.purge(gd);
                observer.disconnect();
            }}
    }});
    
    // Listen for the removal of the full notebook cells
    var notebookContainer = gd.closest('#notebook-container');
    if (notebookContainer) {{
        x.observe(notebookContainer, {childList: true});
    }}
    
    // Listen for the clearing of the current output cell
    var outputEl = gd.closest('.output');
    if (outputEl) {{
        x.observe(outputEl, {childList: true});
    }}
    
                            })                };                });            </script>        </div>


There is an increase of prevalence of hypertension as your body mass
index increase. This is also similar to the population without HIV as
obesity increase your risk of having high blood pressure.

3.2.6. Prevalence of hypertension by BMI category among men and women living with HIV
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: ipython3

    prev_bmi_sex = hiv2.groupby(["bmi_cat2","sex"])["bp_cat2"].value_counts(normalize = True)
    prev_bmi_sex = pd.DataFrame(round(prev_bmi_sex*100, 2))
    prev_bmi_sex.columns = ["Prevalence of Hypertension"]
    prev_bmi_sex = prev_bmi_sex.reset_index()
    prev_bmi_sex = prev_bmi_sex.loc[prev_bmi_sex["bp_cat2"]== "High"].drop(['bp_cat2'], axis = 1)
    prev_bmi_sex 




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>bmi_cat2</th>
          <th>sex</th>
          <th>Prevalence of Hypertension</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>1</th>
          <td>Underweight</td>
          <td>male</td>
          <td>16.36</td>
        </tr>
        <tr>
          <th>3</th>
          <td>Underweight</td>
          <td>female</td>
          <td>10.59</td>
        </tr>
        <tr>
          <th>5</th>
          <td>Normal weight</td>
          <td>male</td>
          <td>20.01</td>
        </tr>
        <tr>
          <th>7</th>
          <td>Normal weight</td>
          <td>female</td>
          <td>17.06</td>
        </tr>
        <tr>
          <th>9</th>
          <td>Overweight</td>
          <td>male</td>
          <td>31.01</td>
        </tr>
        <tr>
          <th>11</th>
          <td>Overweight</td>
          <td>female</td>
          <td>22.37</td>
        </tr>
        <tr>
          <th>12</th>
          <td>Obese</td>
          <td>male</td>
          <td>53.90</td>
        </tr>
        <tr>
          <th>15</th>
          <td>Obese</td>
          <td>female</td>
          <td>29.66</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython3

    fig_prev_bmi_sex = px.bar(prev_bmi_sex,
                            x = "bmi_cat2",
                            y = "Prevalence of Hypertension", 
                      color = "sex",
                barmode = "group",
               facet_col = "sex",
                template="simple_white",
                title = "Prevalence of hypertension by BMI category and gender",
                       text_auto='.3s'
                           )
    fig_prev_bmi_sex.update_yaxes(title_text = "Prevalence of Hypertension")
    fig_prev_bmi_sex.update_xaxes(title_text = "Body mass index category")
    #fig_prev.update_xaxes(categoryorder = 'array', categoryarray= ['2013','2009','2015','2008','2016', '2012', '2014', '2010', '2011'])
    fig_prev_bmi_sex.update_traces(textfont_size = 12, textangle = 0, textposition = "outside", cliponaxis = False)
    fig_prev_bmi_sex.update_yaxes(range= [0, 60])
    fig_prev_bmi_sex.update_layout(showlegend=False)
    fig_prev_bmi_sex.show()



.. raw:: html

    <div>                            <div id="db993f7d-b436-4d6d-b56f-dbf4231f51d9" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("db993f7d-b436-4d6d-b56f-dbf4231f51d9")) {                    Plotly.newPlot(                        "db993f7d-b436-4d6d-b56f-dbf4231f51d9",                        [{"alignmentgroup":"True","hovertemplate":"sex=male<br>bmi_cat2=%{x}<br>Prevalence of Hypertension=%{y}<extra></extra>","legendgroup":"male","marker":{"color":"#1F77B4","pattern":{"shape":""}},"name":"male","offsetgroup":"male","orientation":"v","showlegend":true,"textposition":"outside","texttemplate":"%{y:.3s}","x":["Underweight","Normal weight","Overweight","Obese"],"xaxis":"x","y":[16.36,20.01,31.01,53.9],"yaxis":"y","type":"bar","textfont":{"size":12},"cliponaxis":false,"textangle":0},{"alignmentgroup":"True","hovertemplate":"sex=female<br>bmi_cat2=%{x}<br>Prevalence of Hypertension=%{y}<extra></extra>","legendgroup":"female","marker":{"color":"#FF7F0E","pattern":{"shape":""}},"name":"female","offsetgroup":"female","orientation":"v","showlegend":true,"textposition":"outside","texttemplate":"%{y:.3s}","x":["Underweight","Normal weight","Overweight","Obese"],"xaxis":"x2","y":[10.59,17.06,22.37,29.66],"yaxis":"y2","type":"bar","textfont":{"size":12},"cliponaxis":false,"textangle":0}],                        {"template":{"data":{"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"bar":[{"error_x":{"color":"rgb(36,36,36)"},"error_y":{"color":"rgb(36,36,36)"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"carpet":[{"aaxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"baxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"choropleth"}],"contourcarpet":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"contourcarpet"}],"contour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"contour"}],"heatmapgl":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmapgl"}],"heatmap":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmap"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2dcontour"}],"histogram2d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2d"}],"histogram":[{"marker":{"line":{"color":"white","width":0.6}},"type":"histogram"}],"mesh3d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattermapbox"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolargl"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolar"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"rgb(237,237,237)"},"line":{"color":"white"}},"header":{"fill":{"color":"rgb(217,217,217)"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"colorscale":{"diverging":[[0.0,"rgb(103,0,31)"],[0.1,"rgb(178,24,43)"],[0.2,"rgb(214,96,77)"],[0.3,"rgb(244,165,130)"],[0.4,"rgb(253,219,199)"],[0.5,"rgb(247,247,247)"],[0.6,"rgb(209,229,240)"],[0.7,"rgb(146,197,222)"],[0.8,"rgb(67,147,195)"],[0.9,"rgb(33,102,172)"],[1.0,"rgb(5,48,97)"]],"sequential":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"sequentialminus":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]]},"colorway":["#1F77B4","#FF7F0E","#2CA02C","#D62728","#9467BD","#8C564B","#E377C2","#7F7F7F","#BCBD22","#17BECF"],"font":{"color":"rgb(36,36,36)"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"white","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"angularaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","radialaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"zaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"}},"shapedefaults":{"fillcolor":"black","line":{"width":0},"opacity":0.3},"ternary":{"aaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"baxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","caxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"}}},"xaxis":{"anchor":"y","domain":[0.0,0.49],"title":{"text":"Body mass index category"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Prevalence of Hypertension"},"range":[0,60]},"xaxis2":{"anchor":"y2","domain":[0.51,1.0],"matches":"x","title":{"text":"Body mass index category"}},"yaxis2":{"anchor":"x2","domain":[0.0,1.0],"matches":"y","showticklabels":false,"title":{"text":"Prevalence of Hypertension"},"range":[0,60]},"annotations":[{"font":{},"showarrow":false,"text":"sex=male","x":0.245,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"},{"font":{},"showarrow":false,"text":"sex=female","x":0.755,"xanchor":"center","xref":"paper","y":1.0,"yanchor":"bottom","yref":"paper"}],"legend":{"title":{"text":"sex"},"tracegroupgap":0},"title":{"text":"Prevalence of hypertension by BMI category and gender"},"barmode":"group","showlegend":false},                        {"responsive": true}                    ).then(function(){
    
    var gd = document.getElementById('db993f7d-b436-4d6d-b56f-dbf4231f51d9');
    var x = new MutationObserver(function (mutations, observer) {{
            var display = window.getComputedStyle(gd).display;
            if (!display || display === 'none') {{
                console.log([gd, 'removed!']);
                Plotly.purge(gd);
                observer.disconnect();
            }}
    }});
    
    // Listen for the removal of the full notebook cells
    var notebookContainer = gd.closest('#notebook-container');
    if (notebookContainer) {{
        x.observe(notebookContainer, {childList: true});
    }}
    
    // Listen for the clearing of the current output cell
    var outputEl = gd.closest('.output');
    if (outputEl) {{
        x.observe(outputEl, {childList: true});
    }}
    
                            })                };                });            </script>        </div>


-  Either among men and women, there is an increase prevalence of
   hypertension as your body mass index increase.
-  However, men’s prevalence of hypertension seems to be higher than the
   one of women among overweight and obese PLWHIV.

3.2.7. Prevalence of hypertension among PLWHIV on ART treatment or not
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: ipython3

    ## let's see the proportion of hypertension among PLWHIV on ART and those not on ART
    prev_trt1 = hiv2.groupby("trt")["bp_cat2"].value_counts(
    )
    
    prev_trt = hiv2.groupby("trt")["bp_cat2"].value_counts(
        normalize = True
    )
    prev_trt = pd.DataFrame(round(prev_trt * 100, 2))
    prev_trt.columns = ["Prevalence of Hypertension"]
    prev_trt = prev_trt.reset_index()
    prev_trt = prev_trt.loc[prev_trt["bp_cat2"] == "High"].drop(['bp_cat2'], axis = 1)
    print(prev_trt1) 
    prev_trt


.. parsed-literal::

    trt         bp_cat2
    Not on ART  Normal     1060
                High        284
    On ART      Normal      865
                High        215
    Name: bp_cat2, dtype: int64
    



.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>trt</th>
          <th>Prevalence of Hypertension</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>1</th>
          <td>Not on ART</td>
          <td>21.13</td>
        </tr>
        <tr>
          <th>3</th>
          <td>On ART</td>
          <td>19.91</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython3

    fig_prev_trt = px.bar(prev_trt,
                            x = "trt",
                            y = "Prevalence of Hypertension", 
                     # color = "sex",
                barmode = "group",
               # facet_col = "year",
                template="simple_white",
                title = "Prevalence of hypertension of PLWHIV on ART treatment or not",
                       text_auto='.3s'
                           )
    fig_prev_trt.update_yaxes(title_text = "Prevalence of Hypertension")
    fig_prev_trt.update_xaxes(title_text = "Treatment Group")
    fig_prev_trt.update_traces(textfont_size = 12, textangle = 0, textposition = "outside", cliponaxis = False)
    fig_prev_trt.update_yaxes(range= [0, 30])
    fig_prev_trt.show()



.. raw:: html

    <div>                            <div id="f1444ca2-e547-40b8-bd3e-c1bd0af5dfc2" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("f1444ca2-e547-40b8-bd3e-c1bd0af5dfc2")) {                    Plotly.newPlot(                        "f1444ca2-e547-40b8-bd3e-c1bd0af5dfc2",                        [{"alignmentgroup":"True","hovertemplate":"trt=%{x}<br>Prevalence of Hypertension=%{y}<extra></extra>","legendgroup":"","marker":{"color":"#1F77B4","pattern":{"shape":""}},"name":"","offsetgroup":"","orientation":"v","showlegend":false,"textposition":"outside","texttemplate":"%{y:.3s}","x":["Not on ART","On ART"],"xaxis":"x","y":[21.13,19.91],"yaxis":"y","type":"bar","textfont":{"size":12},"cliponaxis":false,"textangle":0}],                        {"template":{"data":{"barpolar":[{"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"bar":[{"error_x":{"color":"rgb(36,36,36)"},"error_y":{"color":"rgb(36,36,36)"},"marker":{"line":{"color":"white","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"carpet":[{"aaxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"baxis":{"endlinecolor":"rgb(36,36,36)","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"rgb(36,36,36)"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"choropleth"}],"contourcarpet":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"contourcarpet"}],"contour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"contour"}],"heatmapgl":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmapgl"}],"heatmap":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"heatmap"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2dcontour"}],"histogram2d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"histogram2d"}],"histogram":[{"marker":{"line":{"color":"white","width":0.6}},"type":"histogram"}],"mesh3d":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scattermapbox"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolargl"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterpolar"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatter"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"},"colorscale":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"rgb(237,237,237)"},"line":{"color":"white"}},"header":{"fill":{"color":"rgb(217,217,217)"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":1,"tickcolor":"rgb(36,36,36)","ticks":"outside"}},"colorscale":{"diverging":[[0.0,"rgb(103,0,31)"],[0.1,"rgb(178,24,43)"],[0.2,"rgb(214,96,77)"],[0.3,"rgb(244,165,130)"],[0.4,"rgb(253,219,199)"],[0.5,"rgb(247,247,247)"],[0.6,"rgb(209,229,240)"],[0.7,"rgb(146,197,222)"],[0.8,"rgb(67,147,195)"],[0.9,"rgb(33,102,172)"],[1.0,"rgb(5,48,97)"]],"sequential":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]],"sequentialminus":[[0.0,"#440154"],[0.1111111111111111,"#482878"],[0.2222222222222222,"#3e4989"],[0.3333333333333333,"#31688e"],[0.4444444444444444,"#26828e"],[0.5555555555555556,"#1f9e89"],[0.6666666666666666,"#35b779"],[0.7777777777777778,"#6ece58"],[0.8888888888888888,"#b5de2b"],[1.0,"#fde725"]]},"colorway":["#1F77B4","#FF7F0E","#2CA02C","#D62728","#9467BD","#8C564B","#E377C2","#7F7F7F","#BCBD22","#17BECF"],"font":{"color":"rgb(36,36,36)"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"white","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"white","polar":{"angularaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","radialaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"scene":{"xaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"zaxis":{"backgroundcolor":"white","gridcolor":"rgb(232,232,232)","gridwidth":2,"linecolor":"rgb(36,36,36)","showbackground":true,"showgrid":false,"showline":true,"ticks":"outside","zeroline":false,"zerolinecolor":"rgb(36,36,36)"}},"shapedefaults":{"fillcolor":"black","line":{"width":0},"opacity":0.3},"ternary":{"aaxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"baxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"},"bgcolor":"white","caxis":{"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside"}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"},"yaxis":{"automargin":true,"gridcolor":"rgb(232,232,232)","linecolor":"rgb(36,36,36)","showgrid":false,"showline":true,"ticks":"outside","title":{"standoff":15},"zeroline":false,"zerolinecolor":"rgb(36,36,36)"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Treatment Group"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Prevalence of Hypertension"},"range":[0,30]},"legend":{"tracegroupgap":0},"title":{"text":"Prevalence of hypertension of PLWHIV on ART treatment or not"},"barmode":"group"},                        {"responsive": true}                    ).then(function(){
    
    var gd = document.getElementById('f1444ca2-e547-40b8-bd3e-c1bd0af5dfc2');
    var x = new MutationObserver(function (mutations, observer) {{
            var display = window.getComputedStyle(gd).display;
            if (!display || display === 'none') {{
                console.log([gd, 'removed!']);
                Plotly.purge(gd);
                observer.disconnect();
            }}
    }});
    
    // Listen for the removal of the full notebook cells
    var notebookContainer = gd.closest('#notebook-container');
    if (notebookContainer) {{
        x.observe(notebookContainer, {childList: true});
    }}
    
    // Listen for the clearing of the current output cell
    var outputEl = gd.closest('.output');
    if (outputEl) {{
        x.observe(outputEl, {childList: true});
    }}
    
                            })                };                });            </script>        </div>


-  The prevalence of hypertension is almost similar among PLWHIV on ART
   and those who are not on ART.
-  This does not confirm the fact that the toxicity of ART could lead to
   an increased blood pressure among PLWHIV on ART treatment. However,
   this is a small sample and it would be biased to draw a conclusion on
   the larger population of PLWHIV on ART treament.

Conclusion
----------

The prevalence of high blood pressure is high among people living with
HIV in Cape Town (24 %) with similar distribution among men and women.

However, the prevalence of hypertension is higher in PLWHIV with older
age regarless of there gender. In addition the higher your body mass
index, the higher the prevalence of hypertension and obese PLWHIV have
the highest prevalence of hypertension.

Those who have there HIV controlled and those with not controlled HIV
have similar prevalence of hypertension and this is similar either you
are on ART treatment or not.

If we had more time, we coould have done some inferatial statistics
including data modelling in order to find significant associated factors
to the prevalence of hypertension in this population.

Nevertheless, this descriptive analysis has already proven that there is
a need to strenghten the screening of hypertension in HIV clinics in our
settings and at least among those who are older and also among
overweight and obese patients. This also means that the monitorring of
the weight of these patients is also critical.

Systematically screening all HIV patients for hypertension would allow
referral for hypertension care and allow them to live a healthier life
even after viral suppression.

References
----------

1.  Global burden of 369 diseases and injuries in 204 countries and
    territories, 1990-2019: a systematic analysis for the Global Burden
    of Disease Study 2019. Lancet, 2020. 396(10258): p. 1204-1222.
2.  Institute for Health Metrics and Evaluation. GBD Compare. 2019
    [cited 2021 26 September]; Available from:
    https://vizhub.healthdata.org/gbd-compare/.
3.  World Health Organization, Global status report on noncommunicable
    diseases 2014. Geneva: World Health Organization, 2014.
4.  Guwatudde D, Nankya-Mutyoba J, Kalyesubula R, Laurence C, Adebamowo
    C, Ajayi IO, et al. The burden of hypertension in sub-Saharan
    Africa: A four-country cross sectional study. BMC Public Health.
    2015;15: 1–8. pmid:25563658
5.  Yoruk A, Boulos PK, Bisognano JD. The State of Hypertension in
    Sub-Saharan Africa: Review and Commentary. American Journal of
    Hypertension. Oxford Academic; 2018. pp. 387–388. pmid:29136102
6.  Hendriks ME, Wit FWNM, Roos MTL, Brewster LM, Akande TM, de Beer IH,
    et al. Hypertension in Sub-Saharan Africa: Cross-sectional surveys
    in four rural and urban communities. PLoS One. 2012;7. pmid:22427857
7.  Peer N, Steyn K, Lombard C, Gwebushe N, Levitt N (2013) A High
    Burden of Hypertension in the Urban Black Population of Cape Town:
    The Cardiovascular Risk in Black South Africans (CRIBSA) Study. PLOS
    ONE 8(11): e78567. https://doi.org/10.1371/journal.pone.0078567
8.  Collaboration ATC. Survival of HIV-positive patients starting
    antiretroviral therapy between 1996 and 2013: a collaborative
    analysis of cohort studies. lancet HIV. 2017;4: e349–e356.
    pmid:28501495
9.  Ploth DW, Mbwambo JK, Fonner VA, Horowitz B, Zager P, Schrader R, et
    al. Prevalence of CKD, Diabetes, and Hypertension in Rural Tanzania.
    Kidney Int Reports. 2018;3. pmid:29989050
10. The Joint United Nations Programme on HIV/AIDS U. Global AIDS. In:
    Aids [Internet]. 2016 pp. S3-11.
    https://doi.org/10.1073/pnas.86.15.5781
11. Chepchirchir A. International Journal of Cytokine Expression and
    Hypertension Comorbidity in HIV / AIDS Patients at Kenyatta National
    Hospital HIV Care. 2018. pmid:28990417
12. Lubega G, Mayanja B, Lutaakome J, Abaasa A, Thomson R, Lindan C.
    Prevalence and factors associated with hypertension among people
    living with HIV/AIDS on antiretroviral therapy in Uganda. PAMJ 2021;
    38:216. 2021;38. pmid:34046122
13. French MA, King MS, Tschampa JM, da Silva BA, Landay AL. Serum
    Immune Activation Markers Are Persistently Increased in Patients
    with HIV Infection after 6 Years of Antiretroviral Therapy despite
    Suppression of Viral Replication and Reconstitution of CD4 + T
    Cells. J Infect Dis. 2009;200: 1212–1215. pmid:19728788
