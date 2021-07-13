# *This documentation is to answer number 2.*

# *Import the necessary library*
import plotly.graph_objects as go
import dash

import dash_html_components as html
import dash_core_components as dcc
import plotly.offline as pyo


import numpy as np
import pandas as pd
app = dash.Dash(__name__)

# *Load the appropriate data*
df = pd.read_csv("/Users/gerfiazka/Desktop/LuxuryLoanPortfolio.csv")

# *Develop the dashboard*
app.layout = html.Div(children=[html.Div("<b> Luxury Loan Dashboard",style= {"color": "darkblue",
                                                      "text-align": "center", "background-color": "lightblue",
                                                        "border-style": "line", "display": "inline-block", "width":"100%"
                                                                   
                                           }),
                      
                        html.Div(dcc.Dropdown(id="purpose",
    options=[
        {'label': 'Boat', 'value': 'Boat'},
        {'label': 'Investment Property', 'value': 'investment property'},
        {'label': 'Commercial Property', 'value': 'commercial property'},
          {'label': 'Home', 'value': 'home'}
    ],
    value='Boat', style= {"text-align": "center", "border-style": "line", "width":"30%"} 

    
)),
                               html.Div(children=[dcc.Graph(id="plot_area_1",
            figure={
                "df": [
                    {
                        "x": df["purpose"],
                        
                        "type": "piechart",
                    },
                ],
                "layout": {"title": "Property Value for Loan Purpose"},
            }, 
                                                            style= {"color": "lightblue", "text-align":"center", 
                                                                    "background-color":"yellow"}
                                                            ),], style= {"width":"100%", 'paffing':10})
                               ]),
 html.Div(children=[dcc.Graph(id="plot_area_2",
            figure={
                "df": [ {
                        "x": df["interest rate percent"],
                        
                        "type": "line",
                    }, ],
                "layout": {"title": "Interest Rate for the Loans"},
            }, 
                                                            style= {"color": "lightblue", "text-align":"center", 
                                                                    "background-color":"yellow"}),], 
                                                            style= {"width":"100%", 'paffing':10}),
         html.Div(children=[dcc.Graph(id="plot_area_3",
            figure={
                "df": [
                    {
                        "x": df["loan balance"],
                        
                        "type": "line",
                    }, ],
                "layout": {"title": "Loan balance for the Loans"},
            }, 
                                                            style= {"color": "lightblue", "text-align":"center", 
                                                                    "background-color":"yellow"}
                                                        ),], style= {"width":"100%", 'paffing':10}) 
@app.callback([output("plot_area", "children"),
               output("plot_area_2", "children"),
               output("plot_area_3", "children"),
              input("purpose", "value")
              ])

def update_output_plot(input_cat):
    
    df = pd.read_csv("/Users/gerfiazka/Desktop/LuxuryLoanPortfolio.csv")
    

if __name__ == '__main__':
    app.run_server()
