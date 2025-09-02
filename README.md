# Daily-Tracker-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daily Finance Tracker</title>
    <style>
        * {
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            margin: 0;
            padding: 20px;
            background-color: #f5f7fa;
            color: #333;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
            padding: 20px;
        }
        
        h1 {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 30px;
        }
        
        .summary-container {
            display: flex;
            justify-content: space-between;
            margin-bottom: 30px;
            gap: 20px;
        }
        
        .summary-box {
            flex: 1;
            background-color: #f8f9fa;
            border: 1px solid #e9ecef;
            border-radius: 8px;
            padding: 20px;
            text-align: center;
        }
        
        .summary-box.income {
            border-top: 5px solid #27ae60;
        }
        
        .summary-box.expense {
            border-top: 5px solid #e74c3c;
        }
        
        .summary-box.cash {
            border-top: 5px solid #3498db;
        }
        
        .summary-title {
            font-size: 1.2rem;
            margin-bottom: 10px;
            font-weight: bold;
        }
        
        .summary-value {
            font-size: 1.8rem;
            font-weight: bold;
        }
        
        .income-value {
            color: #27ae60;
        }
        
        .expense-value {
            color: #e74c3c;
        }
        
        .cash-value {
            color: #3498db;
        }
        
        .section-title {
            font-size: 1.5rem;
            margin: 30px 0 20px;
            color: #3498db;
            font-weight: bold;
            padding-bottom: 10px;
            border-bottom: 2px solid #3498db;
        }
        
        .month-header {
            text-align: center;
            font-size: 1.5rem;
            margin-bottom: 20px;
            color: #3498db;
            font-weight: bold;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
            margin-bottom: 30px;
        }
        
        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        
        th {
            background-color: #3498db;
            color: white;
        }
        
        tr:nth-child(even) {
            background-color: #f2f2f2;
        }
        
        tr:hover {
            background-color: #e9f7fe;
        }
        
        input[type="text"], input[type="number"] {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        
        .add-row-btn {
            background-color: #2ecc71;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            margin-top: 15px;
            font-size: 16px;
            display: block;
            width: 100%;
        }
        
        .add-row-btn:hover {
            background-color: #27ae60;
        }
        
        .save-btn {
            background-color: #3498db;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 4px;
            cursor: pointer;
        }
        
        .save-btn:hover {
            background-color: #2980b9;
        }
        
        .footer {
            text-align: center;
            margin-top: 30px;
            color: #7f8c8d;
            font-size: 0.9rem;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Daily Finance Tracker</h1>
        
        <div class="month-header" id="current-month"></div>
        
        <div class="summary-container">
            <div class="summary-box income">
                <div class="summary-title">Total Income</div>
                <div class="summary-value income-value" id="total-income">Rs. 0.00</div>
            </div>
            
            <div class="summary-box expense">
                <div class="summary-title">Total Expenditures</div>
                <div class="summary-value expense-value" id="total-expense">Rs. 0.00</div>
            </div>
            
            <div class="summary-box cash">
                <div class="summary-title">Cash in Hand</div>
                <div class="summary-value cash-value" id="cash-in-hand">Rs. 0.00</div>
            </div>
        </div>
        
        <div class="section-title">Income Entries</div>
        
        <table id="income-table">
            <thead>
                <tr>
                    <th>S.No</th>
                    <th>Detail</th>
                    <th>Amount (Rs.)</th>
                    <th>Date</th>
                    <th>Time</th>
                    <th>Date Wise Total</th>
                </tr>
            </thead>
            <tbody>
                <!-- Income rows will be added dynamically -->
            </tbody>
        </table>
        
        <button class="add-row-btn" id="add-income-row">Add New Income</button>
        
        <div class="section-title">Expenditure Entries</div>
        
        <table id="expense-table">
            <thead>
                <tr>
                    <th>S.No</th>
                    <th>Detail</th>
                    <th>Amount (Rs.)</th>
                    <th>Date</th>
                    <th>Time</th>
                    <th>Date Wise Total</th>
                </tr>
            </thead>
            <tbody>
                <!-- Expense rows will be added dynamically -->
            </tbody>
        </table>
        
        <button class="add-row-btn" id="add-expense-row">Add New Expenditure</button>
        
        <div class="footer">
            <p>Entries automatically record date and time when amount is entered</p>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Get current month and year
            const now = new Date();
            const monthNames = ["January", "February", "March", "April", "May", "June",
                "July", "August", "September", "October", "November", "December"
            ];
            document.getElementById('current-month').textContent = 
                `${monthNames[now.getMonth()]} ${now.getFullYear()}`;
            
            // Table references
            const incomeTableBody = document.querySelector('#income-table tbody');
            const expenseTableBody = document.querySelector('#expense-table tbody');
            const addIncomeBtn = document.getElementById('add-income-row');
            const addExpenseBtn = document.getElementById('add-expense-row');
            
            // Summary elements
            const totalIncomeElement = document.getElementById('total-income');
            const totalExpenseElement = document.getElementById('total-expense');
            const cashInHandElement = document.getElementById('cash-in-hand');
            
            // Load data from localStorage if available
            let incomes = JSON.parse(localStorage.getItem('incomes')) || [];
            let expenses = JSON.parse(localStorage.getItem('expenses')) || [];
            
            // Function to calculate date-wise totals
            function calculateDateWiseTotals(entries) {
                const dateTotals = {};
                
                entries.forEach(entry => {
                    if (!dateTotals[entry.date]) {
                        dateTotals[entry.date] = 0;
                    }
                    dateTotals[entry.date] += parseFloat(entry.amount);
                });
                
                return dateTotals;
            }
            
            // Function to update summary boxes
            function updateSummary() {
                let totalIncome = 0;
                let totalExpense = 0;
                
                incomes.forEach(income => {
                    totalIncome += parseFloat(income.amount);
                });
                
                expenses.forEach(expense => {
                    totalExpense += parseFloat(expense.amount);
                });
                
                totalIncomeElement.textContent = `Rs. ${totalIncome.toFixed(2)}`;
                totalExpenseElement.textContent = `Rs. ${totalExpense.toFixed(2)}`;
                cashInHandElement.textContent = `Rs. ${(totalIncome - totalExpense).toFixed(2)}`;
            }
            
            // Render income rows
            function renderIncomeRows() {
                incomeTableBody.innerHTML = '';
                const dateTotals = calculateDateWiseTotals(incomes);
                
                incomes.forEach((income, index) => {
                    const row = document.createElement('tr');
                    
                    row.innerHTML = `
                        <td>${index + 1}</td>
                        <td>${income.detail}</td>
                        <td>Rs. ${parseFloat(income.amount).toFixed(2)}</td>
                        <td>${income.date || ''}</td>
                        <td>${income.time || ''}</td>
                        <td>${income.date ? `Rs. ${dateTotals[income.date].toFixed(2)}` : ''}</td>
                    `;
                    
                    incomeTableBody.appendChild(row);
                });
                
                updateSummary();
            }
            
            // Render expense rows
            function renderExpenseRows() {
                expenseTableBody.innerHTML = '';
                const dateTotals = calculateDateWiseTotals(expenses);
                
                expenses.forEach((expense, index) => {
                    const row = document.createElement('tr');
                    
                    row.innerHTML = `
                        <td>${index + 1}</td>
                        <td>${expense.detail}</td>
                        <td>Rs. ${parseFloat(expense.amount).toFixed(2)}</td>
                        <td>${expense.date || ''}</td>
                        <td>${expense.time || ''}</td>
                        <td>${expense.date ? `Rs. ${dateTotals[expense.date].toFixed(2)}` : ''}</td>
                    `;
                    
                    expenseTableBody.appendChild(row);
                });
                
                updateSummary();
            }
            
            // Add new income row
            addIncomeBtn.addEventListener('click', function() {
                const newRow = document.createElement('tr');
                newRow.innerHTML = `
                    <td>${incomes.length + 1}</td>
                    <td><input type="text" class="detail-input" placeholder="Enter income details"></td>
                    <td><input type="number" class="amount-input" placeholder="0.00" step="0.01"></td>
                    <td></td>
                    <td></td>
                    <td></td>
                `;
                
                incomeTableBody.appendChild(newRow);
                
                // Focus on the detail input
                newRow.querySelector('.detail-input').focus();
                
                // Add event to amount input to save when entered
                const amountInput = newRow.querySelector('.amount-input');
                amountInput.addEventListener('blur', function() {
                    if (this.value) {
                        const now = new Date();
                        const dateString = now.toLocaleDateString();
                        const timeString = now.toLocaleTimeString();
                        
                        // Update the date and time cells
                        newRow.cells[3].textContent = dateString;
                        newRow.cells[4].textContent = timeString;
                        
                        // Save to incomes array
                        incomes.push({
                            detail: newRow.querySelector('.detail-input').value,
                            amount: parseFloat(this.value).toFixed(2),
                            date: dateString,
                            time: timeString
                        });
                        
                        // Save to localStorage
                        localStorage.setItem('incomes', JSON.stringify(incomes));
                        
                        // Convert inputs to text
                        newRow.cells[1].textContent = newRow.querySelector('.detail-input').value;
                        newRow.cells[2].textContent = 'Rs. ' + parseFloat(this.value).toFixed(2);
                        
                        // Update the rows and summary
                        renderIncomeRows();
                    }
                });
            });
            
            // Add new expense row
            addExpenseBtn.addEventListener('click', function() {
                const newRow = document.createElement('tr');
                newRow.innerHTML = `
                    <td>${expenses.length + 1}</td>
                    <td><input type="text" class="detail-input" placeholder="Enter expense details"></td>
                    <td><input type="number" class="amount-input" placeholder="0.00" step="0.01"></td>
                    <td></td>
                    <td></td>
                    <td></td>
                `;
                
                expenseTableBody.appendChild(newRow);
                
                // Focus on the detail input
                newRow.querySelector('.detail-input').focus();
                
                // Add event to amount input to save when entered
                const amountInput = newRow.querySelector('.amount-input');
                amountInput.addEventListener('blur', function() {
                    if (this.value) {
                        const now = new Date();
                        const dateString = now.toLocaleDateString();
                        const timeString = now.toLocaleTimeString();
                        
                        // Update the date and time cells
                        newRow.cells[3].textContent = dateString;
                        newRow.cells[4].textContent = timeString;
                        
                        // Save to expenses array
                        expenses.push({
                            detail: newRow.querySelector('.detail-input').value,
                            amount: parseFloat(this.value).toFixed(2),
                            date: dateString,
                            time: timeString
                        });
                        
                        // Save to localStorage
                        localStorage.setItem('expenses', JSON.stringify(expenses));
                        
                        // Convert inputs to text
                        newRow.cells[1].textContent = newRow.querySelector('.detail-input').value;
                        newRow.cells[2].textContent = 'Rs. ' + parseFloat(this.value).toFixed(2);
                        
                        // Update the rows and summary
                        renderExpenseRows();
                    }
                });
            });
            
            // Initial render
            renderIncomeRows();
            renderExpenseRows();
        });
    </script>
</body>
</html>
