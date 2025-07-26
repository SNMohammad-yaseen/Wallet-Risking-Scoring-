Wallet Risk Scoring from Scratch 
Problem Statement
Given a list of wallet addresses, the objective is to build a risk scoring system that assigns a score between 0 to 1000 to each wallet based on its historical transaction behavior with lending protocols like Compound V2/V3. The score should reflect how responsible or risky the wallet's behavior has been, considering factors like borrowing, repaying, and liquidations.
________________________________________
Data Fetching Attempt & Challenges
•	The initial approach involved using The Graph API (Compound V2 Subgraph) to fetch wallet-level transaction data.
•	The Graph Subgraph provides only the current state of wallets (active borrow/supply balances) and does not provide historical transaction events (like past borrows, repays, or liquidations per wallet).
•	For most wallet addresses provided, the Subgraph API returned zero balances and no activity, since many wallets were either inactive or had closed positions.
•	Given API rate limits and the absence of historical logs, fetching actionable data directly from The Graph Subgraph was not feasible within assignment timelines.
________________________________________
Simulated Data Explanation (For Demonstration Purposes)
•	To demonstrate the end-to-end scoring pipeline logic, a simulated dataset was created.
•	Randomized values were generated for:
o	Borrow balances
o	Supply balances
o	Lifetime borrow/supply interest
o	Liquidation counts
•	This approach allowed testing the feature engineering and scoring logic even without real transaction data.
•	In a real deployment scenario, the same pipeline would process actual fetched data from Covalent API, Etherscan logs, or a full DeFi transaction indexer.



Scoring Logic & Feature Engineering
The following features were engineered from wallet transaction data:
1.	Deposit Borrow Ratio: (Supply Balance) / (Borrow Balance + epsilon)
2.	Repayment Efficiency: (Lifetime Supply Interest) / (Lifetime Borrow Interest + epsilon)
3.	Liquidation Penalty: Negative impact based on the number of liquidations
4.	Borrow Exposure: Higher borrow balances reduce the score due to increased risk.
Scoring Formula (Weighted):
makefile
CopyEdit
Score = (
    DepositBorrowRatio * 300 +
    RepaymentEfficiency * 400 -
    LiquidationsCount * 200 -
    BorrowBalance * 0.05
)
Finally, the raw score is normalized to a 0–1000 scale using MinMaxScaler for interpretability.
________________________________________
Production Readiness & API Integration
•	The entire pipeline is modular and designed to plug into real-time data APIs like:
o	The Graph Protocol (Custom Compound Subgraph)
o	Etherscan Event Logs API
o	Covalent API (Unified DeFi Data APIs)
•	By replacing the simulated data generator with actual API responses, this scoring engine can run in production environments, offering live risk scoring.
•	The scoring weights and feature calculations are transparent and can be fine-tuned based on business requirements.


Deliverables
1.	wallet_risk_scores.csv — Final risk scores (wallet, score)
2.	Source Code — Feature extraction, scoring logic script
3.	This README — Explaining the approach, challenges, and solution architecture.
________________________________________
Author
S N Mohammad Yaseen
(LinkedIn : https://www.linkedin.com/in/s-n-mohammad-yaseen )
( GitHub Profile link : https://github.com/SNMohammad-yaseen )

