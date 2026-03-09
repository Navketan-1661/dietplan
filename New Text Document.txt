import streamlit as st
import pandas as pd

st.title("Diet Planner App")

data = pd.read_csv("diet_data.csv")

st.sidebar.header("Filters")

meal = st.sidebar.multiselect(
    "Select Meal Type",
    options=data["Meal_Type"].unique(),
    default=data["Meal_Type"].unique()
)

diet = st.sidebar.multiselect(
    "Select Diet Type",
    options=data["Diet_Type"].unique(),
    default=data["Diet_Type"].unique()
)

filtered = data[
    (data["Meal_Type"].isin(meal)) &
    (data["Diet_Type"].isin(diet))
]

st.subheader("Available Food Items")
st.dataframe(filtered)

st.subheader("Nutrition Summary")

st.write("Total Calories:", filtered["Calories"].sum())
st.write("Total Protein:", filtered["Protein_g"].sum())
st.write("Total Carbs:", filtered["Carbs_g"].sum())
st.write("Total Fat:", filtered["Fat_g"].sum())

st.subheader("Create Your Meal Plan")

foods = st.multiselect(
    "Select Food Items",
    filtered["Food"].unique()
)

plan = filtered[filtered["Food"].isin(foods)]

if not plan.empty:
    st.write("Your Meal Plan")
    st.dataframe(plan)
    st.write("Meal Calories:", plan["Calories"].sum())