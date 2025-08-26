# salehome
โปรแกรมขายฝาก
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>โปรแกรมจัดการขายฝาก</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Sarabun:wght@400;700&display=swap');
        body {
            font-family: 'Sarabun', sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f0f2f5;
            color: #333;
        }
        .container {
            max-width: 1200px;
            margin: auto;
            background: #fff;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
        }
        h2 {
            text-align: center;
            color: #1a237e;
            margin-bottom: 20px;
        }
        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }
        .dashboard-card {
            padding: 25px;
            border-radius: 8px;
            text-align: center;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
            transition: transform 0.3s ease;
        }
        .dashboard-card:hover {
            transform: translateY(-5px);
        }
        .dashboard-card.active-contracts { background-color: #bbdefb; }
        .dashboard-card.inactive-contracts { background-color: #f8bbd0; }
        .dashboard-card.total-interest { background-color: #c8e6c9; }
        .dashboard-card.total-investment { background-color: #ffe0b2; }

        .dashboard-card h3 {
            margin: 0 0 10px;
            color: #3949ab;
            font-size: 1.1em;
        }
        .dashboard-card p {
            font-size: 2em;
            font-weight: bold;
            color: #1a237e;
            margin: 0;
        }
        .form-section, .contracts-section {
            margin-top: 30px;
            border-top: 2px solid #e0e0e0;
            padding-top: 20px;
        }
        .form-section h3, .contracts-section h3 {
            color: #3949ab;
            margin-bottom: 15px;
        }
        .form-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }
        .form-group {
            margin-bottom: 15px;
        }
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
        }
        .form-group input, .form-group select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
        }
        button {
            width: 100%;
            padding: 12px;
            background-color: #43a047;
            color: #fff;
            border: none;
            border-radius: 5px;
            font-size: 1em;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #388e3c;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        table th, table td {
            border: 1px solid #ddd;
            padding: 12px;
            text-align: left;
        }
        table th {
            background-color: #5c6bc0;
            color: white;
            position: sticky;
            top: 0;
        }
        table tr:nth-child(even) {
            background-color: #f9f9f9;
        }
        .status-active {
            color: #43a047;
            font-weight: bold;
        }
        .status-inactive {
            color: #d32f2f;
            font-weight: bold;
        }
        .status-due {
            color: #ff9800; /* Orange for due date */
            font-weight: bold;
        }
        .status-overdue {
            color: #f44336; /* Red for overdue */
            font-weight: bold;
        }
        .action-btn {
            background-color: #fbc02d;
            color: #333;
            padding: 6px 10px;
            font-size: 0.9em;
            border-radius: 4px;
        }
        .action-btn:hover {
            background-color: #f9a825;
        }
        .payment-btn {
            background-color: #3f51b5;
            color: white;
            padding: 6px 10px;
            font-size: 0.9em;
            border-radius: 4px;
            margin-left: 5px;
        }
        .payment-btn:hover {
            background-color: #303f9f;
        }
        .modal {
            display: none;
            position: fixed;
            z-index: 1;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.4);
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            background-color: #fefefe;
            padding: 20px;
            border: 1px solid #888;
            border-radius: 10px;
            max-width: 500px;
            width: 90%;
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
            position: relative;
        }
        .close-btn {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
        }
        .close-btn:hover,
        .close-btn:focus {
            color: black;
            text-decoration: none;
            cursor: pointer;
        }
        #paymentHistoryList li {
            background-color: #e8eaf6;
            padding: 10px;
            border-radius: 5px;
            margin-bottom: 8px;
        }
        .form-payment input, .form-payment select {
            width: calc(100% - 22px);
            margin-bottom: 10px;
        }
        .main-button-group {
            text-align: center;
            margin-bottom: 20px;
        }
        #showFormBtn {
            width: auto;
            padding: 12px 30px;
            background-color: #1a237e;
        }
        #showFormBtn:hover {
            background-color: #283593;
        }
        .edit-btn {
            background-color: #2196f3;
            color: white;
            padding: 6px 10px;
            font-size: 0.9em;
            border-radius: 4px;
            margin-right: 5px;
        }
        .edit-btn:hover {
            background-color: #1e88e5;
        }
        /* Custom Confirmation Modal */
        #customConfirmModal .modal-content {
            text-align: center;
        }
        #customConfirmModal .button-group {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
        }
        #customConfirmModal .confirm-btn {
            background-color: #4CAF50;
            width: 45%;
        }
        #customConfirmModal .cancel-btn {
            background-color: #f44336;
            width: 45%;
        }
        .message-box {
            background-color: #e8f5e9;
            color: #2e7d32;
            padding: 10px;
            border-radius: 5px;
            margin-bottom: 10px;
            display: none;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>โปรแกรมจัดการขายฝาก</h2>

    <div class="dashboard-grid" id="dashboard">
        <!-- Dashboard cards will be rendered here -->
    </div>

    <div class="main-button-group">
        <button id="showFormBtn" onclick="showAddForm()">เพิ่มสัญญาใหม่</button>
    </div>

    <div id="addContractFormSection" class="form-section" style="display: none;">
        <h3>เพิ่มสัญญาใหม่</h3>
        <form id="contractForm">
            <input type="hidden" id="contractIndex">
            <div class="form-grid">
                <div class="form-group">
                    <label for="customerName">ชื่อลูกค้า:</label>
                    <input type="text" id="customerName" required>
                </div>
                <div class="form-group">
                    <label for="propertyName">ทรัพย์สิน:</label>
                    <input type="text" id="propertyName" required>
                </div>
            </div>
            <div class="form-grid">
                <div class="form-group">
                    <label for="contractDate">วันที่ทำสัญญา:</label>
                    <input type="date" id="contractDate" required>
                </div>
                <div class="form-group">
                    <label for="deedNumber">เลขโฉนด:</label>
                    <input type="text" id="deedNumber" required>
                </div>
            </div>
            <!-- New fields for property location -->
            <div class="form-grid">
                <div class="form-group">
                    <label for="subdistrict">ตำบล:</label>
                    <input type="text" id="subdistrict" required>
                </div>
                <div class="form-group">
                    <label for="district">อำเภอ:</label>
                    <input type="text" id="district" required>
                </div>
            </div>
            <div class="form-grid">
                <div class="form-group">
                    <label for="province">จังหวัด:</label>
                    <input type="text" id="province" required>
                </div>
                <!-- This empty div is for layout consistency -->
                <div></div>
            </div>
            <!-- End of new fields -->
            <div class="form-grid">
                <div class="form-group">
                    <label for="pawnPrice">ยอดขายฝาก (บาท):</label>
                    <input type="number" id="pawnPrice" required>
                </div>
                <div class="form-group">
                    <label for="interestRate">ดอกเบี้ย (% ต่อเดือน):</label>
                    <input type="number" id="interestRate" step="0.01" required>
                </div>
            </div>
            <div class="form-grid">
                <div class="form-group">
                    <label for="durationMonths">ระยะเวลาของสัญญา (เดือน):</label>
                    <input type="number" id="durationMonths" required>
                </div>
                <div class="form-group">
                    <label for="prepaidMonths">หักดอกเบี้ยล่วงหน้า (เดือน):</label>
                    <select id="prepaidMonths" required>
                        <option value="0">0 เดือน</option>
                        <option value="1">1 เดือน</option>
                        <option value="2">2 เดือน</option>
                        <option value="3">3 เดือน</option>
                    </select>
                </div>
            </div>
            <div class="form-grid">
                <div class="form-group">
                    <label for="brokerFee">ค่านายหน้า (บาท):</label>
                    <input type="number" id="brokerFee" value="0" required>
                </div>
                <div class="form-group">
                    <label for="landFee">ค่าดำเนินการที่กรมที่ดิน (บาท):</label>
                    <input type="number" id="landFee" value="0" required>
                </div>
            </div>
            <div class="form-grid">
                <div class="form-group">
                    <label for="transferFee">ค่าหักโอน (บาท):</label>
                    <input type="number" id="transferFee" value="0" required>
                </div>
                <!-- This empty div is for layout consistency -->
                <div></div>
            </div>
            <button type="submit" id="submitBtn">บันทึกสัญญา</button>
        </form>
    </div>
    
    <div class="contracts-section">
        <h3>รายการสัญญา</h3>
        <table id="contractsTable">
            <thead>
                <tr>
                    <th>ลูกค้า</th>
                    <th>ทรัพย์สิน</th>
                    <th>เลขโฉนด</th>
                    <th>ยอดขายฝาก</th>
                    <th>ดอกเบี้ย/เดือน</th>
                    <th>ระยะเวลา</th>
                    <th>วันที่ทำสัญญา</th>
                    <th>ครบกำหนด</th>
                    <th>เงินที่ลูกค้าได้รับจริง</th>
                    <th>สถานะ</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <!-- Contract rows will be rendered here -->
            </tbody>
        </table>
    </div>
</div>

<!-- The Modal for Payment History -->
<div id="paymentModal" class="modal">
    <div class="modal-content">
        <span class="close-btn" onclick="closeModal('paymentModal')">&times;</span>
        <h3>ประวัติการชำระดอกเบี้ย</h3>
        <div id="paymentFormSection">
            <h4>เพิ่มประวัติการชำระ</h4>
            <div class="message-box" id="paymentMessageBox"></div>
            <div class="form-payment">
                <label for="paymentDate">วันที่ชำระ:</label>
                <input type="date" id="paymentDate">
                <label for="paymentAmount">ยอดเงิน (บาท):</label>
                <input type="number" id="paymentAmount" step="0.01" required>
                <label for="paymentType">วิธีชำระ:</label>
                <select id="paymentType">
                    <option value="cash">เงินสด</option>
                    <option value="transfer">โอน</option>
                </select>
                <button onclick="addPayment()">บันทึก</button>
            </div>
        </div>
        <ul id="paymentHistoryList">
            <!-- Payment history will be rendered here -->
        </ul>
    </div>
</div>

<!-- Custom Confirmation Modal -->
<div id="customConfirmModal" class="modal">
    <div class="modal-content">
        <p id="confirmMessage"></p>
        <div class="button-group">
            <button class="confirm-btn" id="confirmActionBtn">ยืนยัน</button>
            <button class="cancel-btn" onclick="closeModal('customConfirmModal')">ยกเลิก</button>
        </div>
    </div>
</div>

<script>
    const STORAGE_KEY = 'pawnshop_contracts';
    let contracts = [];
    let currentContractIndex = null;
    let currentActionCallback = null;

    // Function to format a date to DD-MM-YYYY
    function formatDate(dateString) {
        if (!dateString) return '';
        const date = new Date(dateString);
        const day = String(date.getDate()).padStart(2, '0');
        const month = String(date.getMonth() + 1).padStart(2, '0');
        const year = date.getFullYear();
        return `${day}-${month}-${year}`;
    }

    // Function to show custom confirmation modal
    function showConfirmModal(message, callback) {
        document.getElementById('confirmMessage').textContent = message;
        document.getElementById('customConfirmModal').style.display = 'flex';
        currentActionCallback = callback;
    }

    // Function to close any modal
    function closeModal(modalId) {
        document.getElementById(modalId).style.display = 'none';
    }

    // Function to handle the confirmation action
    document.getElementById('confirmActionBtn').addEventListener('click', () => {
        if (currentActionCallback) {
            currentActionCallback();
        }
        closeModal('customConfirmModal');
    });

    // Function to load contracts from local storage
    function loadContracts() {
        const storedContracts = localStorage.getItem(STORAGE_KEY);
        if (storedContracts) {
            contracts = JSON.parse(storedContracts);
        }
        renderDashboard();
        renderContractsTable();
    }

    // Function to save contracts to local storage
    function saveContracts() {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(contracts));
    }

    // Function to show the "Add New Contract" form
    function showAddForm() {
        const formSection = document.getElementById('addContractFormSection');
        const form = document.getElementById('contractForm');
        const submitBtn = document.getElementById('submitBtn');
        form.reset();
        document.getElementById('contractIndex').value = '';
        document.getElementById('contractDate').value = new Date().toISOString().slice(0, 10);
        submitBtn.textContent = 'บันทึกสัญญา';
        formSection.style.display = 'block';
    }

    // Function to edit an existing contract
    function editContract(index) {
        const contract = contracts[index];
        const formSection = document.getElementById('addContractFormSection');
        const form = document.getElementById('contractForm');
        const submitBtn = document.getElementById('submitBtn');

        document.getElementById('customerName').value = contract.customerName;
        document.getElementById('propertyName').value = contract.propertyName;
        document.getElementById('contractDate').value = contract.contractDate;
        document.getElementById('deedNumber').value = contract.deedNumber;
        document.getElementById('subdistrict').value = contract.subdistrict;
        document.getElementById('district').value = contract.district;
        document.getElementById('province').value = contract.province;
        document.getElementById('pawnPrice').value = contract.pawnPrice;
        document.getElementById('interestRate').value = contract.interestRate;
        document.getElementById('durationMonths').value = contract.durationMonths;
        document.getElementById('prepaidMonths').value = contract.prepaidMonths;
        document.getElementById('brokerFee').value = contract.brokerFee;
        document.getElementById('landFee').value = contract.landFee;
        document.getElementById('transferFee').value = contract.transferFee;

        document.getElementById('contractIndex').value = index;
        submitBtn.textContent = 'อัปเดตสัญญา';
        formSection.style.display = 'block';
        window.scrollTo(0, document.body.scrollHeight);
    }

    // Function to calculate the next payment due date
    function getNextPaymentDate(contract) {
        const contractDate = new Date(contract.contractDate);
        const lastPaymentDate = contract.payments && contract.payments.length > 0 ? new Date(contract.payments[contract.payments.length - 1].date) : contractDate;
        const nextPaymentDate = new Date(lastPaymentDate);
        nextPaymentDate.setMonth(nextPaymentDate.getMonth() + 1);
        return nextPaymentDate;
    }

    // Function to get contract status based on payment and due date
    function getContractStatus(contract) {
        if (contract.status === 'inactive') {
            return { text: 'สิ้นสุดแล้ว', class: 'status-inactive' };
        }
        
        const nextPaymentDate = getNextPaymentDate(contract);
        const now = new Date();

        if (now < nextPaymentDate) {
            return { text: 'ยังไม่ถึงกำหนด', class: 'status-active' };
        } else if (now >= nextPaymentDate && now <= new Date(nextPaymentDate.getTime() + (7 * 24 * 60 * 60 * 1000))) { // Example: due for 7 days
            return { text: 'ถึงกำหนดชำระ', class: 'status-due' };
        } else {
            return { text: 'เกินกำหนดชำระ', class: 'status-overdue' };
        }
    }

    // Function to render the dashboard summary
    function renderDashboard() {
        const dashboardDiv = document.getElementById('dashboard');
        const totalInvestment = contracts.reduce((sum, c) => sum + (c.pawnPrice), 0);
        
        let totalInterestCollected = 0;
        contracts.forEach(contract => {
            if (contract.payments && contract.payments.length > 0) {
                const contractInterest = contract.payments.reduce((sum, p) => sum + p.amount, 0);
                totalInterestCollected += contractInterest;
            }
        });

        const activeContracts = contracts.filter(c => c.status === 'active').length;
        const inactiveContracts = contracts.filter(c => c.status === 'inactive').length;

        dashboardDiv.innerHTML = `
            <div class="dashboard-card active-contracts">
                <h3>สัญญาที่มีผลบังคับ</h3>
                <p>${activeContracts}</p>
            </div>
            <div class="dashboard-card inactive-contracts">
                <h3>สัญญาที่สิ้นสุดแล้ว</h3>
                <p>${inactiveContracts}</p>
            </div>
            <div class="dashboard-card total-interest">
                <h3>ดอกเบี้ยรวมที่เก็บได้</h3>
                <p>${totalInterestCollected.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2})} ฿</p>
            </div>
            <div class="dashboard-card total-investment">
                <h3>ยอดขายฝากรวม</h3>
                <p>${totalInvestment.toLocaleString()} ฿</p>
            </div>
        `;
    }

    // Function to render the contracts table
    function renderContractsTable() {
        const tableBody = document.querySelector('#contractsTable tbody');
        tableBody.innerHTML = '';
        
        contracts.forEach((contract, index) => {
            const row = document.createElement('tr');
            const statusInfo = getContractStatus(contract);
            const nextDueDate = getNextPaymentDate(contract);
            
            row.innerHTML = `
                <td>${contract.customerName}</td>
                <td>${contract.propertyName}</td>
                <td>${contract.deedNumber}</td>
                <td>${contract.pawnPrice.toLocaleString()} ฿</td>
                <td>${contract.monthlyInterest.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2})} ฿</td>
                <td>${contract.durationMonths} เดือน</td>
                <td>${formatDate(contract.contractDate)}</td>
                <td>${formatDate(nextDueDate.toISOString().slice(0, 10))}</td>
                <td>${contract.initialReceivedAmount.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2})} ฿</td>
                <td><span class="${statusInfo.class}">${statusInfo.text}</span></td>
                <td>
                    <button class="edit-btn" onclick="editContract(${index})">แก้ไข</button>
                    <button class="payment-btn" onclick="showPaymentHistory(${index})">ดูประวัติ</button>
                </td>
            `;
            tableBody.appendChild(row);
        });
    }

    // Function to end a contract and mark it as inactive
    function endContract(index) {
        showConfirmModal('คุณแน่ใจว่าต้องการปิดสัญญานี้หรือไม่? การปิดสัญญาจะทำให้ไม่สามารถเพิ่มการชำระได้อีก', () => {
            contracts[index].status = 'inactive';
            saveContracts();
            renderDashboard();
            renderContractsTable();
        });
    }

    // Functions for Payment Modal
    function showPaymentHistory(index) {
        currentContractIndex = index;
        const contract = contracts[index];
        const modal = document.getElementById('paymentModal');
        const historyList = document.getElementById('paymentHistoryList');
        
        historyList.innerHTML = '';
        if (contract.payments && contract.payments.length > 0) {
            contract.payments.forEach(payment => {
                const listItem = document.createElement('li');
                listItem.textContent = `วันที่: ${formatDate(payment.date)} - ยอดเงิน: ${payment.amount.toLocaleString()} ฿ - วิธีชำระ: ${payment.type === 'cash' ? 'เงินสด' : 'โอน'}`;
                historyList.appendChild(listItem);
            });
        } else {
            historyList.innerHTML = '<li>ไม่มีประวัติการชำระเงิน</li>';
        }
        
        modal.style.display = 'flex';
    }

    function addPayment() {
        if (currentContractIndex === null) return;
        
        const contract = contracts[currentContractIndex];
        const paymentDate = document.getElementById('paymentDate').value;
        const paymentAmount = parseFloat(document.getElementById('paymentAmount').value);
        const paymentType = document.getElementById('paymentType').value;
        const messageBox = document.getElementById('paymentMessageBox');

        if (!paymentDate) {
            messageBox.textContent = "กรุณาระบุวันที่ชำระเงิน";
            messageBox.style.display = 'block';
            return;
        }

        if (isNaN(paymentAmount) || paymentAmount <= 0) {
            messageBox.textContent = "กรุณาระบุยอดเงินที่ถูกต้อง";
            messageBox.style.display = 'block';
            return;
        }

        if (!contract.payments) {
            contract.payments = [];
        }

        const newPayment = {
            date: paymentDate,
            amount: paymentAmount,
            type: paymentType
        };
        
        contract.payments.push(newPayment);
        saveContracts();
        showPaymentHistory(currentContractIndex); // Reload history
        messageBox.textContent = "บันทึกประวัติการชำระเรียบร้อย";
        messageBox.style.display = 'block';
        setTimeout(() => messageBox.style.display = 'none', 3000);
    }

    // Event listener for form submission
    document.getElementById('contractForm').addEventListener('submit', function(event) {
        event.preventDefault();
        
        const contractIndex = document.getElementById('contractIndex').value;
        
        // Define the save/update logic in a separate function
        const saveOrUpdate = () => {
            const customerName = document.getElementById('customerName').value;
            const propertyName = document.getElementById('propertyName').value;
            const contractDate = document.getElementById('contractDate').value;
            const deedNumber = document.getElementById('deedNumber').value;
            // Get new property location data
            const subdistrict = document.getElementById('subdistrict').value;
            const district = document.getElementById('district').value;
            const province = document.getElementById('province').value;
            const pawnPrice = parseFloat(document.getElementById('pawnPrice').value);
            const interestRate = parseFloat(document.getElementById('interestRate').value);
            const durationMonths = parseInt(document.getElementById('durationMonths').value);
            const prepaidMonths = parseInt(document.getElementById('prepaidMonths').value);
            const brokerFee = parseFloat(document.getElementById('brokerFee').value);
            const landFee = parseFloat(document.getElementById('landFee').value);
            const transferFee = parseFloat(document.getElementById('transferFee').value);
            
            // Calculate monthly and total interest based on the pawn price
            const monthlyInterest = (pawnPrice * (interestRate / 100));
            const totalInterest = monthlyInterest * durationMonths;
            const prepaidInterestAmount = monthlyInterest * prepaidMonths;

            // Calculate the actual amount the customer receives after deductions
            const initialReceivedAmount = pawnPrice - prepaidInterestAmount - brokerFee - transferFee;
            
            const contractData = {
                customerName,
                propertyName,
                contractDate,
                deedNumber,
                subdistrict, // Add new fields to data object
                district,
                province,
                pawnPrice,
                interestRate,
                durationMonths,
                prepaidMonths,
                brokerFee,
                landFee,
                transferFee,
                monthlyInterest,
                totalInterest,
                initialReceivedAmount,
                status: 'active',
                payments: []
            };

            if (contractIndex) {
                // Edit existing contract
                const originalContract = contracts[parseInt(contractIndex)];
                Object.assign(originalContract, contractData);
            } else {
                // Add new contract
                contracts.push(contractData);
            }

            saveContracts();
            
            // Clear the form and hide it
            this.reset();
            document.getElementById('addContractFormSection').style.display = 'none';
            
            // Re-render the UI
            renderDashboard();
            renderContractsTable();
        };

        if (contractIndex) {
            // If it's an edit, show the confirmation modal
            showConfirmModal('คุณแน่ใจที่จะอัปเดตสัญญานี้หรือไม่? การแก้ไขนี้จะแทนที่ข้อมูลเดิม', saveOrUpdate);
        } else {
            // If it's a new contract, save directly
            saveOrUpdate();
        }
    });

    // Initial load
    window.onload = loadContracts;
</script>

</body>
</html>
