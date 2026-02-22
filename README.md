# business-analytics-cafeteria-sales
A Pandas-based analytics script that evaluates product profitability and calculates KPIs to optimize food inventory
import pandas as pd

def analyze_business_metrics():
    print("--- Cafeteria Inventory Business Analytics ---")
    
    # 1. Sales Dataset (Cost and Revenue per item)
    data = {
        'Category': ['Junk Food', 'Healthy Food', 'Junk Food', 'Healthy Food', 'Healthy Food'],
        'Item': ['Chips', 'Fruit Salad', 'Soda', 'Grilled Chicken Wrap', 'Protein Bar'],
        'Units_Sold': [150, 85, 200, 60, 110],
        'Unit_Cost': [10.0, 25.0, 15.0, 40.0, 30.0],
        'Selling_Price': [20.0, 50.0, 30.0, 90.0, 60.0]
    }
    df = pd.DataFrame(data)
    
    # 2. Calculate Key Performance Indicators (KPIs)
    df['Total_Revenue'] = df['Units_Sold'] * df['Selling_Price']
    df['Total_Cost'] = df['Units_Sold'] * df['Unit_Cost']
    df['Total_Profit'] = df['Total_Revenue'] - df['Total_Cost']
    df['Profit_Margin_%'] = (df['Total_Profit'] / df['Total_Revenue']) * 100
    
    # 3. Group Data to extract Business Insights
    category_metrics = df.groupby('Category').agg({
        'Total_Revenue': 'sum',
        'Total_Profit': 'sum'
    }).reset_index()
    
    category_metrics['Overall_Margin_%'] = (category_metrics['Total_Profit'] / category_metrics['Total_Revenue']) * 100
    
    print("\nFinancial Breakdown by Category:")
    print(category_metrics.to_string(index=False))
    
    print("\n--- Actionable Business Insight ---")
    healthy_margin = category_metrics.loc[category_metrics['Category'] == 'Healthy Food', 'Overall_Margin_%'].values[0]
    junk_margin = category_metrics.loc[category_metrics['Category'] == 'Junk Food', 'Overall_Margin_%'].values[0]
    
    if healthy_margin > junk_margin:
        print("Recommendation: Shift inventory budget toward Healthy Food. It yields a higher profit margin per unit sold.")
    else:
        print("Recommendation: Optimize supply chain costs for Healthy Food to improve margins.")

if __name__ == "__main__":
    analyze_business_metrics()
