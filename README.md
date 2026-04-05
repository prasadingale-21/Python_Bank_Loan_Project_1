# Python_Bank_Loan_Project_1

## Summary

### Key Performance Indicators (KPIs) Requirements:

1. **Total Loan Applications**: We need to calculate the total number of loan applications received during a specified period. Additionally, it is essential to monitor the Month-to-Date (MTD) Loan Applications.

    total_loan_application = df['id'].count()
    print("Total Loan Applications:", total_loan_application)

   O/P: Total Loan Applications: 38576


 **Total Funded Amount:** Understanding the total amount of funds disbursed as loans is crucial. We also want to keep an eye on the MTD Total Funded Amount metric.

    total_funded_amount = df['loan_amount'].sum()
    total_funded_amount_millions = total_funded_amount  / 1000000
    print("Total Funded Amount: ${:.2F}M".format(total_funded_amount_millions))


 4. Total Amount Received: Tracking the total amount received from borrowers is essential for assessing the bank's cash flow and loan repayment. We should analyse the Month-to-Date (MTD) Total Amount Receive.  

    total_amount_received = df['total_payment'].sum()
    
    total_amount_received_millions = total_amount_received/1000000
    
    print("Total Amount Received: ${:.2F}M".format(total_amount_received_millions))


5. Average Interest Rate: Calculating the average interest rate across all loans which will provide insights into our lending portfolio's overall cost.

    average_interest_rate = df['int_rate'].mean() * 100
    print("Average Interest Rate: {:.2f}%".format(average_interest_rate))



6. Average Debt-to-Income Ratio (DTI): Evaluating the average DTI for our borrowers helps us gauge their financial health. We need to compute the average DTI for all loans.
   
average_debt_to_income = df['dti'].mean() * 100
print("Average Debt to Incomce Ratio (DTI): {:.2f}%".format(average_debt_to_income))


