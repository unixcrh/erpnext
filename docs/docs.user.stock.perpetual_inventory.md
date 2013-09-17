---
{
	"_label": "Perpetual Inventory"
}
---

In perpetual accounting, system creates accounting entries for each stock transactions. Hence, stock balance will always remains same as relevant account balance.

## **Activation**

1. Setup the following default accounts for each Company 
	- Stock Received But Not Billed
	- Stock Adjustment Account
	- Expenses Included In Valuation
	- Cost Center
	
2. The system will create an account head for each warehouse. Enter "Create Account Under" (account group under which account will be created) in warehouse master, based on type of items it stores (Stores, Fixed Asset Warehouse, etc).

3. Go to Setup > Accounts Settings > check "Make Accounting Entry For Every Stock Movement"
![Activation](img/accounting-for-stock-1.png)


## **Migration from Periodic Inventory**

Migration from Periodic Inventory is not a one click settings, it involves some special steps. As Perpetual Inventory always maintain a sync between stock and account balance, it is not possible to enable it with existing Warehouse setup. You have to create a whole new set of Warehouses, each linked to relevant account. 

Steps to be followed:

- Nullify current stock-in-hand / fixed-asset account balance through Journal Voucher.
- Create new warehouse for each existing warehouse
- Follow Activation Step 1, 2 & 3
- Create Stock Entry (Material Transfer) to transfer available stock from existing warehouse to new warehouse

>Note: System will not  post any accounting entries for existing stock transactions submitted prior to the activation of Perpetual Inventory as those old warehouses will not be linked to account.

-

## **Example**

Consider following Chart of Accounts and Warehouse setup for your company:

#### Chart of Accounts

>- Assets (Dr)
>  - Current Assets
>    - Accounts Receivable
>      - Jane Doe
>    - Stock Assets
>      - Stores
>      - Finished Goods
>      - Work In Progress
>    - Tax Assets
>      - VAT
>  - Fixed Assets
>    - Fixed Asset Warehouse
>- Liabilities (Cr)
>  - Current Liabilities
>    - Accounts Payable
>      - East Wind Inc.
>    - Stock Liabilities
>      - Stock Received But Not Billed
>    - Tax Liabilities
>      - Service Tax
>- Income (Cr)
>  - Direct Income
>    - Sales Account
>- Expenses (Dr)
>  - Direct Expenses
>    - Stock Expenses
>      - Cost of Goods Sold
>      - Expenses Included In Valuation
>      - Stock Adjustment
>      - Shipping Charges
>      - Customs Duty
  
#### Warehouse - Account Configuration

>- Stores
>- Work In Progress
>- Finished Goods
>- Fixed Asset Warehouse

### **Purchase Receipt**

>Suppose you have purchased *10 quantity* of item "RM0001" at *$200* and *5 quantity* of item "Desktop" at **$100** from supplier "East Wind Inc". Following are the details of Purchase Receipt:

><b>Supplier:</b> East Wind Inc.

><b>Items:</b>
><table class="table table-bordered">
>	<thead>
>		<tr>
>			<th>Item</th><th>Warehouse</th><th>Qty</th>
>			<th>Rate</th><th>Amount</th><th>Valuation Amount</th>
>		</tr>
>	</thead>
>	<tbody>
>		<tr>
>			<td>RM0001</td><td>Stores</td><td>10</td><td>200</td><td>2000</td><td>2200</td>
>		</tr>
>		<tr>
>			<td>Desktop</td><td>Fixed Asset Warehouse</td>
>			<td>5</td><td>100</td><td>500</td><td>550</td>
>		</tr>
>	</tbody>
></table>
	
>**Taxes:**

><table class="table table-bordered">
>	<thead>
>		<tr><th>Account</th><th>Amount</th><th>Category</th></tr>
>	</thead>
>	<tbody>
>		<tr><td>Shipping Charges</td><td>100</td><td>Total and Valuation</td></tr>
>		<tr><td>VAT</td><td>120</td><td>Total</td></tr>
>		<tr><td>Customs Duty</td><td>150</td><td>Valuation</td></tr>
>	</tbody>
></table>

**Stock Ledger**

![pr_stock_ledger](img/accounting-for-stock-2.png)

**General Ledger**

![pr_general_ledger](img/accounting-for-stock-3.png)

As stock balance increases through Purchase Receipt, "Store" and "Fixed Asset Warehouse" accounts have been debited and a temporary account "Stock Receipt But Not Billed" account has been credited, to maintain double entry accounting system.


--

### **Purchase Invoice**

>On receiving Bill from supplier, for the above Purchase Receipt, you will make Purchase Invoice for the same. The general ledger entries are as follows:

**General Ledger**

![pi_general_ledger](img/accounting-for-stock-4.png)


Here "Stock Received But Not Billed" account has been debited and nullified the effect of Purchase Receipt. "Expenses Included In Valuation" account has been credited which ensures the valuation expense accounts are not booked (debited) twice (in Purchase Invoice and Delivery Note).

--

### **Delivery Note**

>Lets say, you have an order from "Jane Doe" to deliver 5 qty of item "RM0001" at $300. Following is the details of Delivery Note:

>**Customer:** Jane Doe

>**Items:**
><table class="table table-bordered">
>	<thead>
>		<tr><th>Item</th><th>Warehouse</th><th>Qty</th><th>Rate</th><th>Amount</th></tr>
>	</thead>
>	<tbody>
>		<tr><td>RM0001</td><td>Stores</td><td>5</td><td>300</td><td>1500</td></tr>
>	</tbody>
></table>
	
>**Taxes:**

><table class="table table-bordered">
>	<thead>
>		<tr><th>Account</th><th>Amount</th></tr>
>	</thead>
>	<tbody>
>		<tr><td>Service Tax</td><td>150</td></tr>
>		<tr><td>VAT</td><td>100</td></tr>
>	</tbody>
></table>


**Stock Ledger**

![dn_stock_ledger](img/accounting-for-stock-5.png)

**General Ledger**

![dn_general_ledger](img/accounting-for-stock-6.png)

As item has delivered from "Stores" warehouse, "Stores" account has been credited and equal amount will be debited to the expense account "Cost of Goods Sold". The debit/credit amount is equal to the total buying cost of the selling items. And buying cost is calculated based on valuation method (FIFO / Moving Average) or serial no cost for serialized items.

In this eample, Buying cost of RM0001 = (2200/10)*5 = 1100

--

### **Sales Invoice with Update Stock**

> Suppose you do not want to make Delivery Note against the above order, you can make Sales Invoice directly with "Update Stock" options. The details of the Sales Invoice are same as above Delivery Note.

**Stock Ledger**

![si_stock_ledger](img/accounting-for-stock-7.png)

**General Ledger**

![si_general_ledger](img/accounting-for-stock-8.png)

Here apart from normal account entries for invoice, "Stores" and "Cost of Goods Sold" accounts are also affected based on buying cost.

--

### **Stock Entry (Material Receipt)**

>**Items:**
><table class="table table-bordered">
>	<thead>
>		<tr><th>Item</th><th>Target Warehouse</th><th>Qty</th><th>Rate</th><th>Amount</th></tr>
>	</thead>
>	<tbody>
>		<tr><td>RM0001</td><td>Stores</td><td>50</td><td>220</td><td>11000</td></tr>
>	</tbody>
></table>

**Stock Ledger**

![mr_stock_ledger](img/accounting-for-stock-9.png)

**General Ledger**

![mr_stock_ledger](img/accounting-for-stock-10.png)
 
--

### **Stock Entry (Material Issue)**

>**Items:**
><table class="table table-bordered">
>	<thead>
>		<tr><th>Item</th><th>Source Warehouse</th><th>Qty</th><th>Rate</th><th>Amount</th></tr>
>	</thead>
>	<tbody>
>		<tr><td>RM0001</td><td>Stores</td><td>10</td><td>220</td><td>2200</td></tr>
>	</tbody>
></table>

**Stock Ledger**

![mi_stock_ledger](img/accounting-for-stock-11.png)

**General Ledger**

![mi_stock_ledger](img/accounting-for-stock-12.png)

--

### **Stock Entry (Material Transfer)**

>**Items:**
><table class="table table-bordered">
>	<thead>
>		<tr><th>Item</th><th>Source Warehouse</th><th>Target Warehouse</th>
>		<th>Qty</th><th>Rate</th><th>Amount</th></tr>
>	</thead>
>	<tbody>
>		<tr><td>RM0001</td><td>Stores</td><td>Work In Progress</td>
>		<td>10</td><td>220</td><td>2200</td></tr>
>	</tbody>
></table>

**Stock Ledger**

![mtn_stock_ledger](img/accounting-for-stock-13.png)

**General Ledger**

No General Ledger Entry

--

### **Stock Entry (Sales Return - Sales Invoice booked)**

>**Items:**
><table class="table table-bordered">
>	<thead>
>		<tr><th>Item</th><th>Target Warehouse</th><th>Qty</th><th>Rate</th><th>Amount</th></tr>
>	</thead>
>	<tbody>
>		<tr><td>RM0001</td><td>Stores</td><td>2</td><td>200</td><td>400</td></tr>
>	</tbody>
></table>

**Stock Ledger**

![sret_stock_ledger](img/accounting-for-stock-14.png)

**General Ledger**

![sret_general_ledger](img/accounting-for-stock-15.png)


--

### **Stock Entry (Purchase Return)**

>**Items:**
><table class="table table-bordered">
>	<thead>
>		<tr><th>Item</th><th>Source Warehouse</th><th>Qty</th><th>Rate</th><th>Amount</th></tr>
>	</thead>
>	<tbody>
>		<tr><td>RM0001</td><td>Stores</td><td>4</td><td>220</td><td>880</td></tr>
>	</tbody>
></table>

**Stock Ledger**

![pret_stock_ledger](img/accounting-for-stock-16.png)

**General Ledger**

![pret_general_ledger](img/accounting-for-stock-17.png)