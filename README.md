# StockFlow-Inventory
# StockFlow Inventory Management System

## Overview
This is my submission for the StockFlow case study—a B2B SaaS platform for inventory management. It allows small businesses to track products across multiple warehouses, manage supplier relationships, and handle bundles. The project includes API fixes, database design, and a new low-stock alerts endpoint, built with Python/Flask.

## Assumptions Made Due to Incomplete Requirements
- **Database Schema**: Assumed a relational database (e.g., PostgreSQL) with tables for Companies, Warehouses, Products, Inventory, Suppliers, ProductSuppliers, Bundles, and InventoryChanges. Added `company_id` for multi-tenancy in B2B SaaS.
- **Product Bundles**: Bundles are products containing other products (e.g., kits). Inventory is tracked at the bundle level; sub-products aren't recursively deducted.
- **Low-Stock Alerts**: "Recent sales activity" means sales in the last 30 days. "Days until stockout" is calculated as `current_stock / average_daily_sales` from the `Sales` table. Thresholds are per product.
- **Authentication/Authorization**: Not implemented; assumed a placeholder `get_current_company_id()` function for security.
- **Error Handling**: Basic Flask error handling; in production, add logging and custom exceptions.
- **Data Types**: Prices use `DECIMAL(10,2)` for precision. Quantities are integers. Dates use `TIMESTAMP`.
- **Scalability**: Designed for moderate scale (e.g., 10k products per company); use partitioning for larger datasets.

## Project Structure
- `app.py`: Main Flask application with API endpoints (Parts 1 and 3).
- `models.py`: SQLAlchemy database models (from Part 2).
- `schema.sql`: SQL DDL for database schema (from Part 2).
- `requirements.txt`: Python dependencies.
- `README.md`: This documentation.

## Parts Summary
### Part 1: Code Review & Debugging
- **Issues Identified**: No input validation, no SKU uniqueness check, flawed multi-warehouse logic, missing optional field handling, precision issues with prices, poor transaction management, and lack of error responses.
- **Fixes**: Added validation, SKU checks, single transactions, proper error handling, and security validations. Updated code to reuse products across warehouses and handle optional fields.
- **Impact**: Prevents crashes, data corruption, and business logic violations in production.

### Part 2: Database Design
- **Schema**: Tables with relationships, indexes, and constraints for multi-tenancy, uniqueness, and auditing. Includes self-referencing for bundles and many-to-many for suppliers.
- **Gaps & Questions**: Asked about bundle pricing, sales calculations, supplier prioritization, dynamic thresholds, and data retention.
- **Decisions**: Used foreign keys for integrity, UNIQUE constraints for SKUs, and indexes for performance. Normalized for scalability.

### Part 3: API Implementation
- **Endpoint**: `GET /api/companies/{company_id}/alerts/low-stock` returns low-stock alerts with supplier info.
- **Logic**: Filters products below threshold with recent sales, calculates stockout days, and handles multi-warehouse.
- **Edge Cases**: Handles no sales data, zero averages, missing suppliers, invalid IDs, and DB errors.
- **Approach**: Efficient queries with joins and subqueries; added comments for clarity.

## Setup Instructions
1. **Prerequisites**: Python 3.8+, Git, and a database (e.g., PostgreSQL or SQLite for testing).
2. **Install Dependencies**: Run `pip install -r requirements.txt`.
3. **Database Setup**:
   - Create a database and run `schema.sql` to create tables.
   - In `app.py`, set `app.config['SQLALCHEMY_DATABASE_URI']` (e.g., `'postgresql://user:pass@localhost/stockflow'`).
4. **Run the App**: Execute `python app.py`. Access at `http://127.0.0.1:5000`.
5. **Test Endpoints**: Use Postman or a browser for API calls (see code comments for examples).

## Technologies Used
- **Backend**: Python, Flask, SQLAlchemy.
- **Database**: PostgreSQL (or SQLite for dev).
- **Tools**: Eclipse with PyDev, Git, GitHub.

## Evaluation Notes
- **Technical Skills**: Focused on clean code, best practices, and scalability.
- **Problem-Solving**: Identified ambiguities and made justified assumptions.
- **Business Understanding**: Considered real-world constraints like multi-tenancy and error handling.

## Contact
- **Author**: [Your Name]
- **GitHub**: [Link to your repo, e.g., https://github.com/yourusername/StockFlow-Inventory]
- **Live Session Prep**: Ready to discuss debugging, trade-offs, and edge cases.
