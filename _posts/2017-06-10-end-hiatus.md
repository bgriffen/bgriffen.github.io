---
layout: post
title: "End Hiatus"
share: true
tags:
- dataviz
---
I haven't posted much content over the past year as I've been quite preoccupied with other activities. It's time to change.

The number and quality of data visualization tools available on the web has increased markedly over the past few years. It is time to bring the presentation of my work into the new era. To this end I'm retroactively going to update my previous blog posts to be more easily read and used. 

Starting from today each of my blog posts will be a standalone Jupyter notebook. This hopefully will allow people to work through the same process I did in achieving a given result. Additionally, my posts which contained static figures output from matplotlib will now be rendered dynamically using tools such as [Plotly](https://plot.ly/python/). What does this mean in practice? Let's quickly take a look.


```python
import plotly.plotly as py      # my new goto plotting library (https://plot.ly/)
import plotly.graph_objs as go
import pandas as pd             # for handling data
import math                     # for doing math
```

Time to digest some data.


```python
# import some data about countries

data = pd.read_csv("https://raw.githubusercontent.com/plotly/datasets/master/gapminderDataFiveYear.csv")
df_2007 = data[data['year']==2007]
df_2007 = df_2007.sort_values(['continent', 'country'])
slope = 2.666051223553066e-05
hover_text = []
bubble_size = []

# construct our hover text for the widget

for index, row in df_2007.iterrows():
    hover_text.append(('Country: {country}<br>'+
                      'Life Expectancy: {lifeExp}<br>'+
                      'GDP per capita: {gdp}<br>'+
                      'Population: {pop}<br>'+
                      'Year: {year}').format(country=row['country'],
                                            lifeExp=row['lifeExp'],
                                            gdp=row['gdpPercap'],
                                            pop=row['pop'],
                                            year=row['year']))
    bubble_size.append(math.sqrt(row['pop']*slope))

# set our text size and bubble size

df_2007['text'] = hover_text
df_2007['size'] = bubble_size

# create our first 'trace' which is just a container for data
# color by continent, Africa first

trace0 = go.Scatter(
    x=df_2007['gdpPercap'][df_2007['continent'] == 'Africa'],
    y=df_2007['lifeExp'][df_2007['continent'] == 'Africa'],
    mode='markers',
    name='Africa',
    text=df_2007['text'][df_2007['continent'] == 'Africa'],
    marker=dict(
        symbol='circle',
        sizemode='diameter',
        sizeref=0.85,
        size=df_2007['size'][df_2007['continent'] == 'Africa'],
        line=dict(
            width=2
        ),
    )
)

# Americas second

trace1 = go.Scatter(
    x=df_2007['gdpPercap'][df_2007['continent'] == 'Americas'],
    y=df_2007['lifeExp'][df_2007['continent'] == 'Americas'],
    mode='markers',
    name='Americas',
    text=df_2007['text'][df_2007['continent'] == 'Americas'],
    marker=dict(
        sizemode='diameter',
        sizeref=0.85,
        size=df_2007['size'][df_2007['continent'] == 'Americas'],
        line=dict(
            width=2
        ),
    )
)

# Asia third

trace2 = go.Scatter(
    x=df_2007['gdpPercap'][df_2007['continent'] == 'Asia'],
    y=df_2007['lifeExp'][df_2007['continent'] == 'Asia'],
    mode='markers',
    name='Asia',
    text=df_2007['text'][df_2007['continent'] == 'Asia'],
    marker=dict(
        sizemode='diameter',
        sizeref=0.85,
        size=df_2007['size'][df_2007['continent'] == 'Asia'],
        line=dict(
            width=2
        ),
    )
)

# Europe fourth

trace3 = go.Scatter(
    x=df_2007['gdpPercap'][df_2007['continent'] == 'Europe'],
    y=df_2007['lifeExp'][df_2007['continent'] == 'Europe'],
    mode='markers',
    name='Europe',
    text=df_2007['text'][df_2007['continent'] == 'Europe'],
    marker=dict(
        sizemode='diameter',
        sizeref=0.85,
        size=df_2007['size'][df_2007['continent'] == 'Europe'],
        line=dict(
            width=2
        ),
    )
)

# Oceania fifth

trace4 = go.Scatter(
    x=df_2007['gdpPercap'][df_2007['continent'] == 'Oceania'],
    y=df_2007['lifeExp'][df_2007['continent'] == 'Oceania'],
    mode='markers',
    name='Oceania',
    text=df_2007['text'][df_2007['continent'] == 'Oceania'],
    marker=dict(
        sizemode='diameter',
        sizeref=0.85,
        size=df_2007['size'][df_2007['continent'] == 'Oceania'],
        line=dict(
            width=2
        ),
    )
)

# put all of these together in a list

data = [trace0, trace1, trace2, trace3, trace4]

# construct the layout - ease of understanding being the priority

layout = go.Layout(
    title='Life Expectancy v. Per Capita GDP, 2007',
    xaxis=dict(
        title='GDP per capita (2000 dollars)',
        gridcolor='rgb(255, 255, 255)',
        range=[2.003297660701705, 5.191505530708712],
        type='log',
        zerolinewidth=1,
        ticklen=5,
        gridwidth=2,
    ),
    yaxis=dict(
        title='Life Expectancy (years)',
        gridcolor='rgb(255, 255, 255)',
        range=[36.12621671352166, 91.72921793264332],
        zerolinewidth=1,
        ticklen=5,
        gridwidth=2,
    ),
    paper_bgcolor='rgb(243, 243, 243)',
    plot_bgcolor='rgb(243, 243, 243)',
)

# render using plotly

fig = go.Figure(data=data, layout=layout)
iplot(fig, filename='life-expectancy-per-GDP-2007')
```


<div id="fdd6e35c-98e8-4b48-95be-fb6073fd2d50" style="height: 525px; width: 100%;" class="plotly-graph-div"></div><script type="text/javascript">require(["plotly"], function(Plotly) { window.PLOTLYENV=window.PLOTLYENV || {};window.PLOTLYENV.BASE_URL="https://plot.ly";Plotly.newPlot("fdd6e35c-98e8-4b48-95be-fb6073fd2d50", [{"type": "scatter", "x": [6223.367465, 4797.231267, 1441.284873, 12569.851770000001, 1217.032994, 430.07069160000003, 2042.0952399999999, 706.016537, 1704.0637239999999, 986.1478792000001, 277.55185869999997, 3632.557798, 1544.750112, 2082.4815670000003, 5581.180998, 12154.08975, 641.3695236000001, 690.8055759, 13206.48452, 752.7497265, 1327.60891, 942.6542111, 579.2317429999999, 1463.249282, 1569.331442, 414.5073415, 12057.49928, 1044.770126, 759.3499101, 1042.581557, 1803.1514960000002, 10956.99112, 3820.17523, 823.6856205, 4811.060429, 619.6768923999999, 2013.9773050000001, 7670.122558, 863.0884639000001, 1598.435089, 1712.4721359999999, 862.5407561000001, 926.1410683, 9269.657808, 2602.394995, 4513.480643, 1107.482182, 882.9699437999999, 7092.923025, 1056.3801210000001, 1271.211593, 469.70929810000007], "y": [72.301, 42.731, 56.728, 50.728, 52.295, 49.58, 50.43, 44.74100000000001, 50.651, 65.152, 46.461999999999996, 55.321999999999996, 48.328, 54.791000000000004, 71.33800000000001, 51.57899999999999, 58.04, 52.946999999999996, 56.735, 59.448, 60.022, 56.007, 46.388000000000005, 54.11, 42.592, 45.678000000000004, 73.952, 59.443000000000005, 48.303000000000004, 54.467, 64.164, 72.801, 71.164, 42.082, 52.906000000000006, 56.867, 46.858999999999995, 76.442, 46.242, 65.528, 63.062, 42.568000000000005, 48.159, 49.339, 58.556000000000004, 39.613, 52.516999999999996, 58.42, 73.923, 51.542, 42.38399999999999, 43.486999999999995], "mode": "markers", "name": "Africa", "text": ["Country: Algeria<br>Life Expectancy: 72.301<br>GDP per capita: 6223.367465<br>Population: 33333216.0<br>Year: 2007", "Country: Angola<br>Life Expectancy: 42.731<br>GDP per capita: 4797.231267<br>Population: 12420476.0<br>Year: 2007", "Country: Benin<br>Life Expectancy: 56.728<br>GDP per capita: 1441.284873<br>Population: 8078314.0<br>Year: 2007", "Country: Botswana<br>Life Expectancy: 50.728<br>GDP per capita: 12569.851770000001<br>Population: 1639131.0<br>Year: 2007", "Country: Burkina Faso<br>Life Expectancy: 52.295<br>GDP per capita: 1217.032994<br>Population: 14326203.0<br>Year: 2007", "Country: Burundi<br>Life Expectancy: 49.58<br>GDP per capita: 430.07069160000003<br>Population: 8390505.0<br>Year: 2007", "Country: Cameroon<br>Life Expectancy: 50.43<br>GDP per capita: 2042.0952399999999<br>Population: 17696293.0<br>Year: 2007", "Country: Central African Republic<br>Life Expectancy: 44.74100000000001<br>GDP per capita: 706.016537<br>Population: 4369038.0<br>Year: 2007", "Country: Chad<br>Life Expectancy: 50.651<br>GDP per capita: 1704.0637239999999<br>Population: 10238807.0<br>Year: 2007", "Country: Comoros<br>Life Expectancy: 65.152<br>GDP per capita: 986.1478792000001<br>Population: 710960.0<br>Year: 2007", "Country: Congo, Dem. Rep.<br>Life Expectancy: 46.461999999999996<br>GDP per capita: 277.55185869999997<br>Population: 64606759.0<br>Year: 2007", "Country: Congo, Rep.<br>Life Expectancy: 55.321999999999996<br>GDP per capita: 3632.557798<br>Population: 3800610.0<br>Year: 2007", "Country: Cote d'Ivoire<br>Life Expectancy: 48.328<br>GDP per capita: 1544.750112<br>Population: 18013409.0<br>Year: 2007", "Country: Djibouti<br>Life Expectancy: 54.791000000000004<br>GDP per capita: 2082.4815670000003<br>Population: 496374.0<br>Year: 2007", "Country: Egypt<br>Life Expectancy: 71.33800000000001<br>GDP per capita: 5581.180998<br>Population: 80264543.0<br>Year: 2007", "Country: Equatorial Guinea<br>Life Expectancy: 51.57899999999999<br>GDP per capita: 12154.08975<br>Population: 551201.0<br>Year: 2007", "Country: Eritrea<br>Life Expectancy: 58.04<br>GDP per capita: 641.3695236000001<br>Population: 4906585.0<br>Year: 2007", "Country: Ethiopia<br>Life Expectancy: 52.946999999999996<br>GDP per capita: 690.8055759<br>Population: 76511887.0<br>Year: 2007", "Country: Gabon<br>Life Expectancy: 56.735<br>GDP per capita: 13206.48452<br>Population: 1454867.0<br>Year: 2007", "Country: Gambia<br>Life Expectancy: 59.448<br>GDP per capita: 752.7497265<br>Population: 1688359.0<br>Year: 2007", "Country: Ghana<br>Life Expectancy: 60.022<br>GDP per capita: 1327.60891<br>Population: 22873338.0<br>Year: 2007", "Country: Guinea<br>Life Expectancy: 56.007<br>GDP per capita: 942.6542111<br>Population: 9947814.0<br>Year: 2007", "Country: Guinea-Bissau<br>Life Expectancy: 46.388000000000005<br>GDP per capita: 579.2317429999999<br>Population: 1472041.0<br>Year: 2007", "Country: Kenya<br>Life Expectancy: 54.11<br>GDP per capita: 1463.249282<br>Population: 35610177.0<br>Year: 2007", "Country: Lesotho<br>Life Expectancy: 42.592<br>GDP per capita: 1569.331442<br>Population: 2012649.0<br>Year: 2007", "Country: Liberia<br>Life Expectancy: 45.678000000000004<br>GDP per capita: 414.5073415<br>Population: 3193942.0<br>Year: 2007", "Country: Libya<br>Life Expectancy: 73.952<br>GDP per capita: 12057.49928<br>Population: 6036914.0<br>Year: 2007", "Country: Madagascar<br>Life Expectancy: 59.443000000000005<br>GDP per capita: 1044.770126<br>Population: 19167654.0<br>Year: 2007", "Country: Malawi<br>Life Expectancy: 48.303000000000004<br>GDP per capita: 759.3499101<br>Population: 13327079.0<br>Year: 2007", "Country: Mali<br>Life Expectancy: 54.467<br>GDP per capita: 1042.581557<br>Population: 12031795.0<br>Year: 2007", "Country: Mauritania<br>Life Expectancy: 64.164<br>GDP per capita: 1803.1514960000002<br>Population: 3270065.0<br>Year: 2007", "Country: Mauritius<br>Life Expectancy: 72.801<br>GDP per capita: 10956.99112<br>Population: 1250882.0<br>Year: 2007", "Country: Morocco<br>Life Expectancy: 71.164<br>GDP per capita: 3820.17523<br>Population: 33757175.0<br>Year: 2007", "Country: Mozambique<br>Life Expectancy: 42.082<br>GDP per capita: 823.6856205<br>Population: 19951656.0<br>Year: 2007", "Country: Namibia<br>Life Expectancy: 52.906000000000006<br>GDP per capita: 4811.060429<br>Population: 2055080.0<br>Year: 2007", "Country: Niger<br>Life Expectancy: 56.867<br>GDP per capita: 619.6768923999999<br>Population: 12894865.0<br>Year: 2007", "Country: Nigeria<br>Life Expectancy: 46.858999999999995<br>GDP per capita: 2013.9773050000001<br>Population: 135031164.0<br>Year: 2007", "Country: Reunion<br>Life Expectancy: 76.442<br>GDP per capita: 7670.122558<br>Population: 798094.0<br>Year: 2007", "Country: Rwanda<br>Life Expectancy: 46.242<br>GDP per capita: 863.0884639000001<br>Population: 8860588.0<br>Year: 2007", "Country: Sao Tome and Principe<br>Life Expectancy: 65.528<br>GDP per capita: 1598.435089<br>Population: 199579.0<br>Year: 2007", "Country: Senegal<br>Life Expectancy: 63.062<br>GDP per capita: 1712.4721359999999<br>Population: 12267493.0<br>Year: 2007", "Country: Sierra Leone<br>Life Expectancy: 42.568000000000005<br>GDP per capita: 862.5407561000001<br>Population: 6144562.0<br>Year: 2007", "Country: Somalia<br>Life Expectancy: 48.159<br>GDP per capita: 926.1410683<br>Population: 9118773.0<br>Year: 2007", "Country: South Africa<br>Life Expectancy: 49.339<br>GDP per capita: 9269.657808<br>Population: 43997828.0<br>Year: 2007", "Country: Sudan<br>Life Expectancy: 58.556000000000004<br>GDP per capita: 2602.394995<br>Population: 42292929.0<br>Year: 2007", "Country: Swaziland<br>Life Expectancy: 39.613<br>GDP per capita: 4513.480643<br>Population: 1133066.0<br>Year: 2007", "Country: Tanzania<br>Life Expectancy: 52.516999999999996<br>GDP per capita: 1107.482182<br>Population: 38139640.0<br>Year: 2007", "Country: Togo<br>Life Expectancy: 58.42<br>GDP per capita: 882.9699437999999<br>Population: 5701579.0<br>Year: 2007", "Country: Tunisia<br>Life Expectancy: 73.923<br>GDP per capita: 7092.923025<br>Population: 10276158.0<br>Year: 2007", "Country: Uganda<br>Life Expectancy: 51.542<br>GDP per capita: 1056.3801210000001<br>Population: 29170398.0<br>Year: 2007", "Country: Zambia<br>Life Expectancy: 42.38399999999999<br>GDP per capita: 1271.211593<br>Population: 11746035.0<br>Year: 2007", "Country: Zimbabwe<br>Life Expectancy: 43.486999999999995<br>GDP per capita: 469.70929810000007<br>Population: 12311143.0<br>Year: 2007"], "marker": {"symbol": "circle", "sizemode": "diameter", "sizeref": 0.85, "size": [29.810746602820707, 18.19714956714691, 14.67555754441577, 6.610603004351237, 19.54338533545803, 14.956442130894004, 21.72077890062959, 10.792626698653965, 16.521859438354298, 4.353683242838514, 41.502401000634656, 10.066092062338798, 21.914531960507805, 3.6377994860078937, 46.2589864862037, 3.833445056960741, 11.437310410545445, 45.164655423539315, 6.227961099314107, 6.709136738617593, 24.6944307003913, 16.2853866046767, 6.2646122858244615, 30.8121008634256, 7.325179403286212, 9.227791164226424, 12.686497529335918, 22.605739846185486, 18.849582296257488, 17.91015962555601, 9.337109185582044, 5.774872714286009, 29.99972628415882, 23.063420581238567, 7.4019919943886965, 18.541405181593333, 59.99999999999956, 4.612764339536934, 15.369704446995593, 2.3067029222366227, 18.084735199216677, 12.799108187017435, 15.592022291528659, 34.249155197329664, 33.57902844158731, 5.496191404660484, 31.88765182447172, 12.329112567064373, 16.55196774082303, 27.887232791983845, 17.696194784090487, 18.116881039099077], "line": {"width": 2}}}, {"type": "scatter", "x": [12779.379640000001, 3822.1370840000004, 9065.800825, 36319.235010000004, 13171.63885, 7006.580419, 9645.06142, 8948.102923, 6025.374752000001, 6873.262326000001, 5728.353514, 5186.050003, 1201.637154, 3548.3308460000003, 7320.880262000001, 11977.57496, 2749.320965, 9809.185636, 4172.838464, 7408.905561, 19328.70901, 18008.50924, 42951.65309, 10611.46299, 11415.805690000001], "y": [75.32, 65.554, 72.39, 80.653, 78.553, 72.889, 78.782, 78.273, 72.235, 74.994, 71.878, 70.259, 60.916000000000004, 70.19800000000001, 72.567, 76.195, 72.899, 75.53699999999999, 71.752, 71.421, 78.74600000000001, 69.819, 78.242, 76.384, 73.747], "mode": "markers", "name": "Americas", "text": ["Country: Argentina<br>Life Expectancy: 75.32<br>GDP per capita: 12779.379640000001<br>Population: 40301927.0<br>Year: 2007", "Country: Bolivia<br>Life Expectancy: 65.554<br>GDP per capita: 3822.1370840000004<br>Population: 9119152.0<br>Year: 2007", "Country: Brazil<br>Life Expectancy: 72.39<br>GDP per capita: 9065.800825<br>Population: 190010647.0<br>Year: 2007", "Country: Canada<br>Life Expectancy: 80.653<br>GDP per capita: 36319.235010000004<br>Population: 33390141.0<br>Year: 2007", "Country: Chile<br>Life Expectancy: 78.553<br>GDP per capita: 13171.63885<br>Population: 16284741.0<br>Year: 2007", "Country: Colombia<br>Life Expectancy: 72.889<br>GDP per capita: 7006.580419<br>Population: 44227550.0<br>Year: 2007", "Country: Costa Rica<br>Life Expectancy: 78.782<br>GDP per capita: 9645.06142<br>Population: 4133884.0<br>Year: 2007", "Country: Cuba<br>Life Expectancy: 78.273<br>GDP per capita: 8948.102923<br>Population: 11416987.0<br>Year: 2007", "Country: Dominican Republic<br>Life Expectancy: 72.235<br>GDP per capita: 6025.374752000001<br>Population: 9319622.0<br>Year: 2007", "Country: Ecuador<br>Life Expectancy: 74.994<br>GDP per capita: 6873.262326000001<br>Population: 13755680.0<br>Year: 2007", "Country: El Salvador<br>Life Expectancy: 71.878<br>GDP per capita: 5728.353514<br>Population: 6939688.0<br>Year: 2007", "Country: Guatemala<br>Life Expectancy: 70.259<br>GDP per capita: 5186.050003<br>Population: 12572928.0<br>Year: 2007", "Country: Haiti<br>Life Expectancy: 60.916000000000004<br>GDP per capita: 1201.637154<br>Population: 8502814.0<br>Year: 2007", "Country: Honduras<br>Life Expectancy: 70.19800000000001<br>GDP per capita: 3548.3308460000003<br>Population: 7483763.0<br>Year: 2007", "Country: Jamaica<br>Life Expectancy: 72.567<br>GDP per capita: 7320.880262000001<br>Population: 2780132.0<br>Year: 2007", "Country: Mexico<br>Life Expectancy: 76.195<br>GDP per capita: 11977.57496<br>Population: 108700891.0<br>Year: 2007", "Country: Nicaragua<br>Life Expectancy: 72.899<br>GDP per capita: 2749.320965<br>Population: 5675356.0<br>Year: 2007", "Country: Panama<br>Life Expectancy: 75.53699999999999<br>GDP per capita: 9809.185636<br>Population: 3242173.0<br>Year: 2007", "Country: Paraguay<br>Life Expectancy: 71.752<br>GDP per capita: 4172.838464<br>Population: 6667147.0<br>Year: 2007", "Country: Peru<br>Life Expectancy: 71.421<br>GDP per capita: 7408.905561<br>Population: 28674757.0<br>Year: 2007", "Country: Puerto Rico<br>Life Expectancy: 78.74600000000001<br>GDP per capita: 19328.70901<br>Population: 3942491.0<br>Year: 2007", "Country: Trinidad and Tobago<br>Life Expectancy: 69.819<br>GDP per capita: 18008.50924<br>Population: 1056608.0<br>Year: 2007", "Country: United States<br>Life Expectancy: 78.242<br>GDP per capita: 42951.65309<br>Population: 301139947.0<br>Year: 2007", "Country: Uruguay<br>Life Expectancy: 76.384<br>GDP per capita: 10611.46299<br>Population: 3447496.0<br>Year: 2007", "Country: Venezuela<br>Life Expectancy: 73.747<br>GDP per capita: 11415.805690000001<br>Population: 26084662.0<br>Year: 2007"], "marker": {"sizemode": "diameter", "sizeref": 0.85, "size": [32.779109473854895, 15.592346310727706, 71.17430139611204, 29.83619048532493, 20.83649530710354, 34.338449847402025, 10.498164837830679, 17.446567616766185, 15.762801031590824, 19.150286550024372, 13.60204531806762, 18.30848712429419, 15.056207247625194, 14.125188672343873, 8.609282386029065, 53.83327441758103, 12.300727542669675, 9.297203500849443, 13.33227490601591, 27.649298541723773, 10.252259728663214, 5.307514532449212, 89.60214975992794, 9.587075116527616, 26.371015346601308], "line": {"width": 2}}}, {"type": "scatter", "x": [974.5803384, 29796.048339999998, 1391.253792, 1713.7786859999999, 4959.1148539999995, 39724.97867, 2452.210407, 3540.6515640000002, 11605.71449, 4471.061906, 25523.2771, 31656.06806, 4519.461171, 1593.06548, 23348.139730000003, 47306.98978, 10461.05868, 12451.6558, 3095.7722710000003, 944.0, 1091.359778, 22316.19287, 2605.94758, 3190.481016, 21654.83194, 47143.179639999995, 3970.0954070000003, 4184.548089, 28718.27684, 7458.3963269999995, 2441.576404, 3025.349798, 2280.769906], "y": [43.828, 75.635, 64.062, 59.723, 72.961, 82.208, 64.69800000000001, 70.65, 70.964, 59.545, 80.745, 82.603, 72.535, 67.297, 78.623, 77.58800000000001, 71.993, 74.241, 66.803, 62.068999999999996, 63.785, 75.64, 65.483, 71.688, 72.777, 79.972, 72.396, 74.143, 78.4, 70.616, 74.249, 73.422, 62.698], "mode": "markers", "name": "Asia", "text": ["Country: Afghanistan<br>Life Expectancy: 43.828<br>GDP per capita: 974.5803384<br>Population: 31889923.0<br>Year: 2007", "Country: Bahrain<br>Life Expectancy: 75.635<br>GDP per capita: 29796.048339999998<br>Population: 708573.0<br>Year: 2007", "Country: Bangladesh<br>Life Expectancy: 64.062<br>GDP per capita: 1391.253792<br>Population: 150448339.0<br>Year: 2007", "Country: Cambodia<br>Life Expectancy: 59.723<br>GDP per capita: 1713.7786859999999<br>Population: 14131858.0<br>Year: 2007", "Country: China<br>Life Expectancy: 72.961<br>GDP per capita: 4959.1148539999995<br>Population: 1318683096.0<br>Year: 2007", "Country: Hong Kong, China<br>Life Expectancy: 82.208<br>GDP per capita: 39724.97867<br>Population: 6980412.0<br>Year: 2007", "Country: India<br>Life Expectancy: 64.69800000000001<br>GDP per capita: 2452.210407<br>Population: 1110396331.0<br>Year: 2007", "Country: Indonesia<br>Life Expectancy: 70.65<br>GDP per capita: 3540.6515640000002<br>Population: 223547000.0<br>Year: 2007", "Country: Iran<br>Life Expectancy: 70.964<br>GDP per capita: 11605.71449<br>Population: 69453570.0<br>Year: 2007", "Country: Iraq<br>Life Expectancy: 59.545<br>GDP per capita: 4471.061906<br>Population: 27499638.0<br>Year: 2007", "Country: Israel<br>Life Expectancy: 80.745<br>GDP per capita: 25523.2771<br>Population: 6426679.0<br>Year: 2007", "Country: Japan<br>Life Expectancy: 82.603<br>GDP per capita: 31656.06806<br>Population: 127467972.0<br>Year: 2007", "Country: Jordan<br>Life Expectancy: 72.535<br>GDP per capita: 4519.461171<br>Population: 6053193.0<br>Year: 2007", "Country: Korea, Dem. Rep.<br>Life Expectancy: 67.297<br>GDP per capita: 1593.06548<br>Population: 23301725.0<br>Year: 2007", "Country: Korea, Rep.<br>Life Expectancy: 78.623<br>GDP per capita: 23348.139730000003<br>Population: 49044790.0<br>Year: 2007", "Country: Kuwait<br>Life Expectancy: 77.58800000000001<br>GDP per capita: 47306.98978<br>Population: 2505559.0<br>Year: 2007", "Country: Lebanon<br>Life Expectancy: 71.993<br>GDP per capita: 10461.05868<br>Population: 3921278.0<br>Year: 2007", "Country: Malaysia<br>Life Expectancy: 74.241<br>GDP per capita: 12451.6558<br>Population: 24821286.0<br>Year: 2007", "Country: Mongolia<br>Life Expectancy: 66.803<br>GDP per capita: 3095.7722710000003<br>Population: 2874127.0<br>Year: 2007", "Country: Myanmar<br>Life Expectancy: 62.068999999999996<br>GDP per capita: 944.0<br>Population: 47761980.0<br>Year: 2007", "Country: Nepal<br>Life Expectancy: 63.785<br>GDP per capita: 1091.359778<br>Population: 28901790.0<br>Year: 2007", "Country: Oman<br>Life Expectancy: 75.64<br>GDP per capita: 22316.19287<br>Population: 3204897.0<br>Year: 2007", "Country: Pakistan<br>Life Expectancy: 65.483<br>GDP per capita: 2605.94758<br>Population: 169270617.0<br>Year: 2007", "Country: Philippines<br>Life Expectancy: 71.688<br>GDP per capita: 3190.481016<br>Population: 91077287.0<br>Year: 2007", "Country: Saudi Arabia<br>Life Expectancy: 72.777<br>GDP per capita: 21654.83194<br>Population: 27601038.0<br>Year: 2007", "Country: Singapore<br>Life Expectancy: 79.972<br>GDP per capita: 47143.179639999995<br>Population: 4553009.0<br>Year: 2007", "Country: Sri Lanka<br>Life Expectancy: 72.396<br>GDP per capita: 3970.0954070000003<br>Population: 20378239.0<br>Year: 2007", "Country: Syria<br>Life Expectancy: 74.143<br>GDP per capita: 4184.548089<br>Population: 19314747.0<br>Year: 2007", "Country: Taiwan<br>Life Expectancy: 78.4<br>GDP per capita: 28718.27684<br>Population: 23174294.0<br>Year: 2007", "Country: Thailand<br>Life Expectancy: 70.616<br>GDP per capita: 7458.3963269999995<br>Population: 65068149.0<br>Year: 2007", "Country: Vietnam<br>Life Expectancy: 74.249<br>GDP per capita: 2441.576404<br>Population: 85262356.0<br>Year: 2007", "Country: West Bank and Gaza<br>Life Expectancy: 73.422<br>GDP per capita: 3025.349798<br>Population: 4018332.0<br>Year: 2007", "Country: Yemen, Rep.<br>Life Expectancy: 62.698<br>GDP per capita: 2280.769906<br>Population: 22211743.0<br>Year: 2007"], "marker": {"sizemode": "diameter", "sizeref": 0.85, "size": [29.158218092531488, 4.346368499824499, 63.33269126387071, 19.410372822791988, 187.50137817012293, 13.641897211716744, 172.05735953138958, 77.2002430612506, 43.0310092001836, 27.076824691452725, 13.089635369762137, 58.295466608856294, 12.703591068691109, 24.924604800707886, 36.1601883828615, 8.173095275129489, 10.224640829775742, 25.724466935285673, 8.753610572213562, 35.684154076889236, 27.75853969364631, 9.243602959999716, 67.17768495299852, 49.27643579280504, 27.126699233639663, 11.01751115964859, 23.308674140715684, 22.692312546756426, 24.85635831606844, 41.65033232229764, 47.677437906919565, 10.350400448892032, 24.33467579451106], "line": {"width": 2}}}, {"type": "scatter", "x": [5937.029525999999, 36126.4927, 33692.60508, 7446.298803, 10680.79282, 14619.222719999998, 22833.30851, 35278.41874, 33207.0844, 30470.0167, 32170.37442, 27538.41188, 18008.94444, 36180.789189999996, 40675.99635, 28569.7197, 9253.896111, 36797.93332, 49357.19017, 15389.924680000002, 20509.64777, 10808.47561, 9786.534714, 18678.31435, 25768.25759, 28821.0637, 33859.74835, 37506.419069999996, 8458.276384, 33203.26128], "y": [76.423, 79.829, 79.441, 74.852, 73.005, 75.748, 76.486, 78.332, 79.313, 80.657, 79.406, 79.483, 73.33800000000001, 81.757, 78.885, 80.546, 74.543, 79.762, 80.196, 75.563, 78.098, 72.476, 74.002, 74.663, 77.926, 80.941, 80.884, 81.70100000000001, 71.777, 79.425], "mode": "markers", "name": "Europe", "text": ["Country: Albania<br>Life Expectancy: 76.423<br>GDP per capita: 5937.029525999999<br>Population: 3600523.0<br>Year: 2007", "Country: Austria<br>Life Expectancy: 79.829<br>GDP per capita: 36126.4927<br>Population: 8199783.0<br>Year: 2007", "Country: Belgium<br>Life Expectancy: 79.441<br>GDP per capita: 33692.60508<br>Population: 10392226.0<br>Year: 2007", "Country: Bosnia and Herzegovina<br>Life Expectancy: 74.852<br>GDP per capita: 7446.298803<br>Population: 4552198.0<br>Year: 2007", "Country: Bulgaria<br>Life Expectancy: 73.005<br>GDP per capita: 10680.79282<br>Population: 7322858.0<br>Year: 2007", "Country: Croatia<br>Life Expectancy: 75.748<br>GDP per capita: 14619.222719999998<br>Population: 4493312.0<br>Year: 2007", "Country: Czech Republic<br>Life Expectancy: 76.486<br>GDP per capita: 22833.30851<br>Population: 10228744.0<br>Year: 2007", "Country: Denmark<br>Life Expectancy: 78.332<br>GDP per capita: 35278.41874<br>Population: 5468120.0<br>Year: 2007", "Country: Finland<br>Life Expectancy: 79.313<br>GDP per capita: 33207.0844<br>Population: 5238460.0<br>Year: 2007", "Country: France<br>Life Expectancy: 80.657<br>GDP per capita: 30470.0167<br>Population: 61083916.0<br>Year: 2007", "Country: Germany<br>Life Expectancy: 79.406<br>GDP per capita: 32170.37442<br>Population: 82400996.0<br>Year: 2007", "Country: Greece<br>Life Expectancy: 79.483<br>GDP per capita: 27538.41188<br>Population: 10706290.0<br>Year: 2007", "Country: Hungary<br>Life Expectancy: 73.33800000000001<br>GDP per capita: 18008.94444<br>Population: 9956108.0<br>Year: 2007", "Country: Iceland<br>Life Expectancy: 81.757<br>GDP per capita: 36180.789189999996<br>Population: 301931.0<br>Year: 2007", "Country: Ireland<br>Life Expectancy: 78.885<br>GDP per capita: 40675.99635<br>Population: 4109086.0<br>Year: 2007", "Country: Italy<br>Life Expectancy: 80.546<br>GDP per capita: 28569.7197<br>Population: 58147733.0<br>Year: 2007", "Country: Montenegro<br>Life Expectancy: 74.543<br>GDP per capita: 9253.896111<br>Population: 684736.0<br>Year: 2007", "Country: Netherlands<br>Life Expectancy: 79.762<br>GDP per capita: 36797.93332<br>Population: 16570613.0<br>Year: 2007", "Country: Norway<br>Life Expectancy: 80.196<br>GDP per capita: 49357.19017<br>Population: 4627926.0<br>Year: 2007", "Country: Poland<br>Life Expectancy: 75.563<br>GDP per capita: 15389.924680000002<br>Population: 38518241.0<br>Year: 2007", "Country: Portugal<br>Life Expectancy: 78.098<br>GDP per capita: 20509.64777<br>Population: 10642836.0<br>Year: 2007", "Country: Romania<br>Life Expectancy: 72.476<br>GDP per capita: 10808.47561<br>Population: 22276056.0<br>Year: 2007", "Country: Serbia<br>Life Expectancy: 74.002<br>GDP per capita: 9786.534714<br>Population: 10150265.0<br>Year: 2007", "Country: Slovak Republic<br>Life Expectancy: 74.663<br>GDP per capita: 18678.31435<br>Population: 5447502.0<br>Year: 2007", "Country: Slovenia<br>Life Expectancy: 77.926<br>GDP per capita: 25768.25759<br>Population: 2009245.0<br>Year: 2007", "Country: Spain<br>Life Expectancy: 80.941<br>GDP per capita: 28821.0637<br>Population: 40448191.0<br>Year: 2007", "Country: Sweden<br>Life Expectancy: 80.884<br>GDP per capita: 33859.74835<br>Population: 9031088.0<br>Year: 2007", "Country: Switzerland<br>Life Expectancy: 81.70100000000001<br>GDP per capita: 37506.419069999996<br>Population: 7554661.0<br>Year: 2007", "Country: Turkey<br>Life Expectancy: 71.777<br>GDP per capita: 8458.276384<br>Population: 71158647.0<br>Year: 2007", "Country: United Kingdom<br>Life Expectancy: 79.425<br>GDP per capita: 33203.26128<br>Population: 60776238.0<br>Year: 2007"], "marker": {"sizemode": "diameter", "sizeref": 0.85, "size": [9.797539869569787, 14.78547987047415, 16.645181537832496, 11.016529874582023, 13.972513922270881, 10.945044520423695, 16.513738358291583, 12.074058148168325, 11.817784349248296, 40.35503054034437, 46.87059592194143, 16.894826887013064, 16.292174168976487, 2.837187889404931, 10.466629714471022, 39.37319325524499, 4.272635311620724, 21.018587741252823, 11.107784556252902, 32.04553066297419, 16.844686384695507, 24.36988025303707, 16.45026638770262, 12.051273531211452, 7.318982231613819, 32.83853667660271, 15.516875720458488, 14.191938980484178, 43.556009677279974, 40.25326864775733], "line": {"width": 2}}}, {"type": "scatter", "x": [34435.367439999995, 25185.00911], "y": [81.235, 80.204], "mode": "markers", "name": "Oceania", "text": ["Country: Australia<br>Life Expectancy: 81.235<br>GDP per capita: 34435.367439999995<br>Population: 20434176.0<br>Year: 2007", "Country: New Zealand<br>Life Expectancy: 80.204<br>GDP per capita: 25185.00911<br>Population: 4115771.0<br>Year: 2007"], "marker": {"sizemode": "diameter", "sizeref": 0.85, "size": [23.34064264905718, 10.475140242695668], "line": {"width": 2}}}], {"title": "Life Expectancy v. Per Capita GDP, 2007", "xaxis": {"title": "GDP per capita (2000 dollars)", "gridcolor": "rgb(255, 255, 255)", "range": [2.003297660701705, 5.191505530708712], "type": "log", "zerolinewidth": 1, "ticklen": 5, "gridwidth": 2}, "yaxis": {"title": "Life Expectancy (years)", "gridcolor": "rgb(255, 255, 255)", "range": [36.12621671352166, 91.72921793264332], "zerolinewidth": 1, "ticklen": 5, "gridwidth": 2}, "paper_bgcolor": "rgb(243, 243, 243)", "plot_bgcolor": "rgb(243, 243, 243)"}, {"showLink": true, "linkText": "Export to plot.ly"})});</script>


The embedded interactivity adds so much more value to a dataset. [Github rendering Jupyter notebooks](https://help.github.com/articles/working-with-jupyter-notebook-files-on-github/) means presenting data through my blog is a breeze. Speaking of data, I will also make the data available for all my musings so that you can play around with the exact notebook I used seamlessly.

This is all part of my goal to make projects more open source and reproducible. It also allows others to build on whatever I'm working on for their own projects which is how I got started all those years ago.

Anyway, I'm looking forward to posting some of my new musings soon. Stay tuned!
