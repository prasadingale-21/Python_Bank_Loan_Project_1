# Python_Bank_Loan_Project_1

## Summary

### Key Performance Indicators (KPIs) Requirements:

* **Total Loan Applications**: We need to calculate the total number of loan applications received during a specified period. Additionally, it is essential to monitor the Month-to-Date (MTD) Loan Applications.

                total_loan_application = df['id'].count()
                print("Total Loan Applications:", total_loan_application)   
                O/P: Total Loan Applications: 38576

* **Total Funded Amount:** Understanding the total amount of funds disbursed as loans is crucial. We also want to keep an eye on the MTD Total Funded Amount metric.

                total_funded_amount = df['loan_amount'].sum()
                total_funded_amount_millions = total_funded_amount  / 1000000
                print("Total Funded Amount: ${:.2F}M".format(total_funded_amount_millions))   
                O/P: Total Funded Amount: $435.76M

* **Total Amount Received:** Tracking the total amount received from borrowers is essential for assessing the bank's cash flow and loan repayment. We should analyse the Month-to-Date (MTD) Total Amount Receive.  

                total_amount_received = df['total_payment'].sum()
                total_amount_received_millions = total_amount_received/1000000
                print("Total Amount Received: ${:.2F}M".format(total_amount_received_millions))  
                O/P: Total Amount Received: $473.07M


* **Average Interest Rate:** Calculating the average interest rate across all loans which will provide insights into our lending portfolio's overall cost.

                average_interest_rate = df['int_rate'].mean() * 100
                print("Average Interest Rate: {:.2f}%".format(average_interest_rate))  
                O/P: Average Interest Rate: 12.05%

* **Average Debt-to-Income Ratio (DTI)**: Evaluating the average DTI for our borrowers helps us gauge their financial health. We need to compute the average DTI for all loans.
   
                average_debt_to_income = df['dti'].mean() * 100
                print("Average Debt to Incomce Ratio (DTI): {:.2f}%".format(average_debt_to_income))  
                O/P: Average Debt to Incomce Ratio (DTI): 13.33%

## Summary

### Good Loan vs Bad Loan KPI’s

* Good Loan Application Percentage   
* Good Loan Applications   
* Good Loan Funded Amount   
* Good Loan Total Received Amount

                    good_loan = df[df['loan_status'].isin(['Current', 'Fully Paid'])]
                    
                    total_loan_application = df['id'].count()
                    
                    good_loan_applications = good_loan['id'].count()
                    good_loan_funded_amount = good_loan['loan_amount'].sum()
                    good_loan_received = good_loan['total_payment'].sum()
                    
                    good_loan_funded_amount_millions = good_loan_funded_amount/1000000
                    good_loan_received_millions = good_loan_received/1000000
                    
                    good_loan_percentage = (good_loan_applications/total_loan_application) * 100
                    
                    print("Good Loan Applications:", good_loan_applications)
                    print("Good Loan Total Funded Amount in Millions: ${:.2f}M".format(good_loan_funded_amount_millions))
                    print("Good Loan Total Amount Received in Millions: ${:.2f}M".format(good_loan_received_millions))
                    print("Good Loan Applications in Percentage: {:.2f}%".format(good_loan_percentage))  
                    
                    O/P: 
                    Good Loan Applications: 33243
                    Good Loan Total Funded Amount in Millions: $370.22M
                    Good Loan Total Amount Received in Millions: $435.79M
                    Good Loan Applications in Percentage: 86.18%



* Bad Loan Application Percentage  
* Bad Loan Applications  
* Bad Loan Funded Amount  
* Bad Loan Total Received Amount

                    bad_loan = df[df['loan_status'].isin(['Charged Off'])]
                    
                    total_loan_application = df['id'].count()
                    
                    bad_loan_applications = bad_loan['id'].count()
                    bad_loan_funded_amount = bad_loan['loan_amount'].sum()
                    bad_loan_received = bad_loan['total_payment'].sum()
                    
                    bad_loan_funded_amount_millions = bad_loan_funded_amount/1000000
                    bad_loan_received_millions = bad_loan_received/1000000
                    
                    bad_loan_percentage = (bad_loan_applications/total_loan_application) * 100
                    
                    print("Bad Loan Applications:", bad_loan_applications)
                    print("Bad Loan Total Funded Amount in Millions: ${:.2f}M".format(bad_loan_funded_amount_millions))
                    print("Bad Loan Total Amount Received in Millions: ${:.2f}M".format(bad_loan_received_millions))
                    print("Bad Loan Applications in Percentage: {:.2f}%".format(bad_loan_percentage))   
                    
                    O/P:
                    Bad Loan Applications: 5333
                    Bad Loan Total Funded Amount in Millions: $65.53M
                    Bad Loan Total Amount Received in Millions: $37.28M
                    Bad Loan Applications in Percentage: 13.82%

### Charts 

* Monthly Trends by Issue Date for Total Funded Amount: To identify seasonality and long-term trends in lending activities!


                monthly_funded = (df.sort_values('issue_date')
                                  .assign(month_name= lambda x: x['issue_date'].dt.strftime('%b %Y'))
                                  .groupby('month_name', sort = False)['loan_amount']
                                  .sum()
                                  .div(1000000)
                                  .reset_index(name = 'loan_amount_millions')
                )
                
                plt.figure(figsize = (10, 5))
                plt.fill_between(monthly_funded['month_name'], monthly_funded['loan_amount_millions'], color='skyblue', alpha = 0.5)
                plt.plot(monthly_funded['month_name'], monthly_funded['loan_amount_millions'], color = 'blue', linewidth = 2)
                
                for i, row in monthly_funded.iterrows():
                    plt.text(i, row['loan_amount_millions'] + 0.1, f"{row['loan_amount_millions']:.2f}", ha = 'center', va = 'bottom',
                             fontsize =9 , rotation = 0, color = 'black')
                plt.title('Total Funded Amount by Month', fontsize = 14)
                plt.xlabel('Month')
                plt.ylabel('Funded Amount (in $Million)')
                plt.xticks(ticks = range(len(monthly_funded)), labels = monthly_funded['month_name'], rotation = 45)
                plt.grid(True, linestyle= '--', alpha=0.6)
                plt.tight_layout()
                plt.show()

  <img width="1367" height="590" alt="image" src="https://github.com/user-attachments/assets/31fb91f9-0dd8-443f-a489-c75fd71efdcb" />

* Monthly Trends by Issue Date for Total Received Amount

                  monthly_received = (df.sort_values('issue_date')
                                  .assign(month_name= lambda x: x['issue_date'].dt.strftime('%b %Y'))
                                  .groupby('month_name', sort = False)['total_payment']
                                  .sum()
                                  .div(1000000)
                                  .reset_index(name = 'received_amount_millions')
                )
                
                plt.figure(figsize = (10, 5))
                plt.fill_between(monthly_received['month_name'], monthly_received['received_amount_millions'], color='lightgreen', alpha = 0.5)
                plt.plot(monthly_received['month_name'], monthly_received['received_amount_millions'], color = 'green', linewidth = 2)
                
                for i, row in monthly_received.iterrows():
                    plt.text(i, row['received_amount_millions'] + 0.1, f"{row['received_amount_millions']:.2f}", ha = 'center', va = 'bottom',
                             fontsize =9 , rotation = 0, color = 'black')
                plt.title('Total received Amount by Month', fontsize = 14)
                plt.xlabel('Month')
                plt.ylabel('Received Amount (in $Million)')
                plt.xticks(ticks = range(len(monthly_received)), labels = monthly_received['month_name'], rotation = 45)
                plt.grid(True, linestyle= '--', alpha=0.6)
                plt.tight_layout()
                plt.show()

<img width="1263" height="601" alt="image" src="https://github.com/user-attachments/assets/63ecb4be-5361-4481-ad01-e1a9004bf245" />

* Monthly Trends by Issue Date for Total Loan Applications

                monthly_applications = (df.sort_values('issue_date')
                                  .assign(month_name= lambda x: x['issue_date'].dt.strftime('%b %Y'))
                                  .groupby('month_name', sort = False)['id']
                                  .count()
                                  .reset_index(name = 'loan_applications_count')
                )
                
                plt.figure(figsize = (10, 5))
                plt.fill_between(monthly_applications['month_name'], monthly_applications['loan_applications_count'], color='orange', alpha = 0.5)
                plt.plot(monthly_applications['month_name'], monthly_applications['loan_applications_count'], color = 'darkorange', linewidth = 2)
                
                for i, row in monthly_applications.iterrows():
                    plt.text(i, row['loan_applications_count'] + 0.1, f"{row['loan_applications_count']}", ha = 'center', va = 'bottom',
                             fontsize =9 , rotation = 0, color = 'black')
                plt.title('Total Loan Applications by Month', fontsize = 14)
                plt.xlabel('Month')
                plt.ylabel('Number of Loan Applications')
                plt.xticks(ticks = range(len(monthly_applications)), labels = monthly_applications['month_name'], rotation = 45)
                plt.grid(True, linestyle= '--', alpha=0.6)
                plt.tight_layout()
                plt.show()

<img width="1271" height="604" alt="image" src="https://github.com/user-attachments/assets/b9577db7-165d-496b-a8ef-b8552d18ebc0" />

* Regional Analysis by State for Total Funded Amount: To identify regions with significant lending activity and assess regional disparities  

                state_funding = df.groupby('address_state')['loan_amount'].sum().sort_values(ascending=True)
                state_funding_thousands = state_funding/1000
                
                plt.figure(figsize = (10,8))
                bars = plt.barh(state_funding_thousands.index, state_funding_thousands.values, color='lightcoral')
                
                for bar in bars:
                    width = bar.get_width()
                    plt.text(width + 10, bar.get_y() + bar.get_height()/2,
                             f"{width:,.0f}K", va = 'center', fontsize = 9)
                
                plt.title('Total Funded Amount by State ($ thousands)', fontsize = 14)
                plt.xlabel('Funded Amount')
                plt.ylabel('State')
                plt.tight_layout()
                plt.show()

<img width="1289" height="690" alt="image" src="https://github.com/user-attachments/assets/8967550b-f2ed-4fa0-ad10-f5b1cd0d2449" />

* Regional Analysis by State for Total Received Amount  

                state_received_amt = df.groupby('address_state')['total_payment'].sum().sort_values(ascending=True)
                state_received_amt_thousands = state_received_amt/1000
                
                plt.figure(figsize = (10,8))
                bars = plt.barh(state_received_amt_thousands.index, state_received_amt_thousands.values, color='lightgreen')
                
                for bar in bars:
                    width = bar.get_width()
                    plt.text(width + 10, bar.get_y() + bar.get_height()/2,
                             f"{width:,.0f}K", va = 'center', fontsize = 9)
                
                plt.title('Total Received Amount by State ($ thousands)', fontsize = 14)
                plt.xlabel('Received Amount')
                plt.ylabel('State')
                plt.tight_layout()
                plt.show()

<img width="1266" height="711" alt="image" src="https://github.com/user-attachments/assets/0ec0f66d-1f14-45ea-86f2-9e27ae4bf681" />

* Regional Analysis by State for Total Number of Applications

                state_applications = df.groupby('address_state')['id'].count().sort_values(ascending=True)
                
                plt.figure(figsize = (10,8))
                bars = plt.barh(state_applications.index, state_applications.values, color='lightblue')
                
                for bar in bars:
                    width = bar.get_width()
                    plt.text(width + 10, bar.get_y() + bar.get_height()/2,
                             f"{width}", va = 'center', fontsize = 9)
                
                plt.title('Total Number of Applications by State (in thousands)', fontsize = 14)
                plt.xlabel('Number of Applications')
                plt.ylabel('State')
                plt.tight_layout()
                plt.show()

<img width="1274" height="685" alt="image" src="https://github.com/user-attachments/assets/4d8c227d-622a-403d-8282-439e6901d80a" />

* Loan Term Analysis (Donut Chart): To allow the client to understand the distribution of loans across various term lengths.
* Loan Term Analysis: for Total Funded Amount

                term_funding_millions =  df.groupby('term')['loan_amount'].sum() / 1000000
                
                plt.figure(figsize = (5, 5))
                plt.pie(term_funding_millions,
                        labels = term_funding_millions.index,
                        autopct = lambda p : f"{p:.1f}%\n${p * sum(term_funding_millions) / 100:.1f}M",
                        startangle = 90,
                        wedgeprops = {'width': 0.4}
                )
                plt.gca().add_artist(plt.Circle((0,0), 0.70, color = 'white'))
                plt.title('Total Funded Amount by Term (in $ Millions)')        
                plt.show()

<img width="666" height="491" alt="image" src="https://github.com/user-attachments/assets/c2708650-a46a-4539-acdf-a384598f0b9b" />

* Loan Term Analysis: for Total Received Amount

                term_received_millions =  df.groupby('term')['total_payment'].sum() / 1000000
                
                plt.figure(figsize = (5, 5))
                plt.pie(term_received_millions,
                        labels = term_received_millions.index,
                        autopct = lambda p : f"{p:.1f}%\n${p * sum(term_received_millions) / 100:.1f}M",
                        startangle = 90,
                        wedgeprops = {'width': 0.4}
                )
                plt.gca().add_artist(plt.Circle((0,0), 0.70, color = 'white'))
                plt.title('Total Received Amount by Term (in $ Millions)')        
                plt.show()

<img width="634" height="470" alt="image" src="https://github.com/user-attachments/assets/75e49396-88ea-4967-8e14-28c3c2a69c3a" />

* Loan Term Analysis: for Total Applications

                term_applications = df.groupby('term')['id'].count()
                plt.figure(figsize=(5, 5))
                plt.pie(term_applications,
                        labels=term_applications.index,
                        autopct=lambda p: f"{p:.1f}%\n{p * sum(term_applications) / 100:.0f}",
                        startangle=90,
                        wedgeprops={'width': 0.4}
                )
                plt.gca().add_artist(plt.Circle((0, 0), 0.70, color='white'))
                plt.title('Total Loan Applications by Term')
                plt.show()

<img width="557" height="484" alt="image" src="https://github.com/user-attachments/assets/d168e329-b0fc-4967-b23a-8d636fea7c9e" />

* Employee Length Analysis (Bar Chart): How lending metrics are distributed among borrowers with different employment lengths, helping us assess the impact of employment history on loan applications.  
* Employee Length: for Total Funded Amount

                emp_funding = df.groupby('emp_length')['loan_amount'].sum().sort_values() / 1000000
                
                plt.figure(figsize=(10, 6))
                bars = plt.barh(emp_funding.index, emp_funding, color='purple')
                
                for bar in bars:
                    width = bar.get_width()
                    plt.text(width + 5, bar.get_y() + bar.get_height() / 2,
                             f"${width:.2f}M", va='center', fontsize=9)
                
                plt.xlabel("Funded Amount ($ Millions)")
                plt.title("Total Funded Amount by Employment Length")
                plt.grid(axis='x', linestyle='--', alpha=0.5)
                plt.tight_layout()
                plt.show()
<img width="1250" height="718" alt="image" src="https://github.com/user-attachments/assets/ccc27be3-bab5-4012-a85f-bdb5da8292e7" />

* Employee Length: for Total Received Amount

                emp_funding_received = df.groupby('emp_length')['total_payment'].sum().sort_values() / 1000000
                
                plt.figure(figsize=(10, 6))
                bars = plt.barh(emp_funding_received.index, emp_funding_received, color='purple')
                
                for bar in bars:
                    width = bar.get_width()
                    plt.text(width + 5, bar.get_y() + bar.get_height() / 2,
                             f"${width:.2f}M", va='center', fontsize=9)
                
                plt.xlabel("Received Amount ($ Millions)")
                plt.title("Total Received Amount by Employment Length")
                plt.grid(axis='x', linestyle='--', alpha=0.5)
                plt.tight_layout()
                plt.show()
<img width="1274" height="709" alt="image" src="https://github.com/user-attachments/assets/97fbdb33-3bdf-4299-a3ae-5f4bbbf79b14" />

* Employee Length: for Total Loan Applications

                emp_loan_applications = df.groupby('emp_length')['id'].count().sort_values()
                
                plt.figure(figsize=(10, 6))
                bars = plt.barh(emp_loan_applications.index, emp_loan_applications, color='purple')
                
                for bar in bars:
                    width = bar.get_width()
                    plt.text(width + 5, bar.get_y() + bar.get_height() / 2,
                             f"{width:.0f}", va='center', fontsize=9)
                
                plt.xlabel("Total Loan Applications")
                plt.title("Total Loan Applications by Employment Length")
                plt.grid(axis='x', linestyle='--', alpha=0.5)
                plt.tight_layout()
                plt.show()

<img width="1260" height="719" alt="image" src="https://github.com/user-attachments/assets/b556536f-453b-414b-b615-a0849185662a" />

* Loan Purpose Breakdown (Bar Chart): Will provide a visual breakdown of loan metrics based on the stated purposes of loans, aiding in the understanding of the primary reasons borrowers seek financing.
* Loan Purpose: for Total Funded Amount  

                emp_funding_purpose = df.groupby('purpose')['loan_amount'].sum().sort_values() / 1000000
                
                plt.figure(figsize=(10, 6))
                bars = plt.barh(emp_funding_purpose.index, emp_funding_purpose, color='blue')
                
                for bar in bars:
                    width = bar.get_width()
                    plt.text(width + 5, bar.get_y() + bar.get_height() / 2,
                             f"${width:.2f}M", va='center', fontsize=9)
                
                plt.xlabel("Funded Amount ($ Millions)")
                plt.title("Total Funded Amount by Purpose")
                plt.grid(axis='x', linestyle='--', alpha=0.5)
                plt.tight_layout()
                plt.show()

<img width="1240" height="719" alt="image" src="https://github.com/user-attachments/assets/58ecf8cc-93aa-4c27-93ed-778b47bd822e" />

* Loan Purpose: for Total Received Amount

                emp_funding_received_purpose = df.groupby('purpose')['total_payment'].sum().sort_values() / 1000000
                
                plt.figure(figsize=(10, 6))
                bars = plt.barh(emp_funding_received_purpose .index, emp_funding_received_purpose, color='blue')
                
                for bar in bars:
                    width = bar.get_width()
                    plt.text(width + 5, bar.get_y() + bar.get_height() / 2,
                             f"${width:.2f}M", va='center', fontsize=9)
                
                plt.xlabel("Received Amount ($ Millions)")
                plt.title("Total Received Amount by Purpose")
                plt.grid(axis='x', linestyle='--', alpha=0.5)
                plt.tight_layout()
                plt.show()

<img width="1226" height="718" alt="image" src="https://github.com/user-attachments/assets/aeb53f0c-184b-4cb9-bf5e-b1dd85c34c52" />

* Loan Purpose: for Total Applications

emp_loan_applications_purpose = df.groupby('purpose')['id'].count().sort_values()

                plt.figure(figsize=(10, 6))
                bars = plt.barh(emp_loan_applications_purpose.index, emp_loan_applications_purpose, color='blue')
                
                for bar in bars:
                    width = bar.get_width()
                    plt.text(width + 5, bar.get_y() + bar.get_height() / 2,
                             f"{width:.0f}", va='center', fontsize=9)
                
                plt.xlabel("Total Loan Applications")
                plt.title("Total Loan Applications by Purpose")
                plt.grid(axis='x', linestyle='--', alpha=0.5)
                plt.tight_layout()
                plt.show()

<img width="1245" height="724" alt="image" src="https://github.com/user-attachments/assets/96b28fcc-668e-4d71-b521-105d07983ecd" />

* Home Ownership Analysis (Tree/ Heat Map): For a hierarchical view of how home ownership impacts loan applications and disbursements.
* Home Ownership by Total Funded Amount  
                
                home_funding = df.groupby('home_ownership')['loan_amount'].sum().reset_index()
                home_funding['loan_amount_millions'] = home_funding['loan_amount'] / 1000000
                
                fig = px.treemap(
                      home_funding,
                      path = ['home_ownership'],
                      values = 'loan_amount_millions',
                      color = 'loan_amount_millions',
                      color_continuous_scale = 'Blues',
                      title = 'Total Funded Amount by Home Ownership ($ Millions)'   
                )
                fig.show()

<img width="1345" height="390" alt="image" src="https://github.com/user-attachments/assets/052fe4bc-480d-47dc-9272-82d3a2f292eb" />

* Home Ownership by Total Received Amount  

                home_funding_received = df.groupby('home_ownership')['total_payment'].sum().reset_index()
                home_funding_received['received_amount_millions'] = home_funding_received['total_payment'] / 1000000
                
                fig = px.treemap(
                      home_funding_received,
                      path = ['home_ownership'],
                      values = 'received_amount_millions',
                      color = 'received_amount_millions',
                      color_continuous_scale = 'blues',
                      title = 'Total Funded Amount received by Home Ownership ($ Millions)'   
                )
                fig.show()

<img width="1305" height="349" alt="image" src="https://github.com/user-attachments/assets/031fd027-7738-463e-9d2d-14e598f83c3b" />

* Home Ownership by Total Applications

                home_ownership_applications = df.groupby('home_ownership')['id'].count().reset_index()
                home_ownership_applications['applications_received_thousands'] = home_ownership_applications['id'] / 1000
                
                fig = px.treemap(
                      home_ownership_applications,
                      path = ['home_ownership'],
                      values = 'applications_received_thousands',
                      color = 'applications_received_thousands',
                      color_continuous_scale = 'blues',
                      title = 'Total Applications Received by Home Ownership (in Thousands)'   
                )
                fig.show()

  <img width="1333" height="353" alt="image" src="https://github.com/user-attachments/assets/925db66f-e2cd-4762-be7d-713f0d6fac16" />

