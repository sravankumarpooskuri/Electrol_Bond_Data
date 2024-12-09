# 📊 Electoral Bond Data Analysis Project

## 🚀 Project Overview
In this project, we analyze the Electoral Bond data following the Supreme Court's landmark judgment mandating transparency in political party funding. The Election Commission of India (ECI) released comprehensive data, which is supplemented by additional data from the Enforcement Directorate. This analysis focuses on understanding the flow of money from donors to recipients, identifying sources of monetary donations, and assessing the impact of political funding transparency.

## 🗂️ Data Tables
We have three main data tables:
1. **Donor Data**: Details about companies, organizations, and individuals who bought electoral bonds.
2. **Receiver Data**: Details about political parties and the bonds they received.
3. **Bank Data**: Details about bank branches where bonds can be issued or encashed.

## 📜 Data Dictionary
### Donor Data Table
| Column Name    | Description                                            |
|----------------|--------------------------------------------------------|
| SNo            | Serial number.                                         |
| Urn            | Unique reference number.                               |
| JournalDate    | Date of journal entry.                                 |
| PurchaseDate   | Date of bond purchase.                                 |
| ExpiryDate     | Expiry date of the bond.                               |
| Purchaser      | Name of the purchaser.                                 |
| Prefix         | Prefix code associated with the bond.                  |
| BondNumber     | Number followed by prefix code.                        |
| Denominations  | Value of the bond.                                     |
| PayBranchCode  | Payment branch code (IFSC).                            |
| PayTeller      | Teller ID who issued the bond.                         |

### Receiver Data Table
| Column Name    | Description                                            |
|----------------|--------------------------------------------------------|
| SNo            | Serial number.                                         |
| DateEncashment | Date of bond encashment.                               |
| PartyName      | Name of the political party (recipient).               |
| AccountNum     | Account number of the party.                           |
| Prefix         | Prefix code associated with the bond.                  |
| BondNumber     | Number followed by prefix code.                        |
| Denominations  | Value of the bond.                                     |
| PayBranchCode  | Payment branch code (IFSC).                            |
| PayTeller      | Teller ID who encashed the bond.                       |

### Bank Branch Data Table
| Column Name               | Description                                                |
|---------------------------|------------------------------------------------------------|
| Sl.No.                    | Serial number.                                             |
| State/UT                  | State/UT where the bank is located.                        |
| Name of the Branch & Address | Complete address of the bank.                             |
| City                      | City where the bank branch is located.                     |
| Branch Code No            | IFSC code.                                                 |

