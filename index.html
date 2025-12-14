# <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Global Daily Savings Vault</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Load Lucide Icons for a clean look -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');
        body { font-family: 'Inter', sans-serif; background-color: #f7f9fc; }
        .active-tab {
            color: #1e40af; /* Blue-700 */
            border-bottom: 2px solid #1e40af;
            font-weight: 600;
        }
        .container-content {
            min-height: calc(100vh - 80px); /* Full height minus navbar height */
            padding-bottom: 20px;
        }
    </style>
</head>
<body>

<div id="app" class="flex flex-col min-h-screen">

        <!-- Main Content Area -->
<main id="content" class="flex-grow container-content max-w-lg mx-auto w-full p-4">
            <!-- Content will be injected here -->
            <div id="loading-spinner" class="flex flex-col items-center justify-center h-48">
                <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-500"></div>
                <p class="mt-4 text-gray-500">Initializing Secure Connection...</p>
            </div>
        </main>

        <!-- Navigation Bar (Fixed at Bottom for Mobile Experience) -->
<nav class="fixed bottom-0 left-0 right-0 bg-white border-t border-gray-200 shadow-xl z-10 max-w-lg mx-auto">
            <div id="nav-bar" class="flex justify-around items-center h-16 text-gray-500">
                <button data-view="home" class="nav-item flex flex-col items-center p-2 text-sm text-blue-700 active-tab">
                    <i data-lucide="home" class="w-5 h-5"></i>
                    <span class="text-xs">Home</span>
                </button>
                <button data-view="deposit" class="nav-item flex flex-col items-center p-2 text-sm">
                    <i data-lucide="plus-circle" class="w-5 h-5"></i>
                    <span class="text-xs">Deposit</span>
                </button>
                <button data-view="balance" class="nav-item flex flex-col items-center p-2 text-sm">
                    <i data-lucide="wallet" class="w-5 h-5"></i>
                    <span class="text-xs">Balance</span>
                </button>
                <button data-view="transactions" class="nav-item flex flex-col items-center p-2 text-sm">
                    <i data-lucide="list-ordered" class="w-5 h-5"></i>
                    <span class="text-xs">History</span>
                </button>
            </div>
        </nav>
    </div>

    <!-- Firebase SDK Imports -->
<script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, setDoc, getDoc, onSnapshot, collection, query, orderBy, limit, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // --- GLOBAL CONFIG & CONSTANTS ---
        const DAILY_INTEREST_RATE = 0.0001; // 0.01% per day
        const CURRENCY_CODE = 'INR'; // Fixed to Rupee for the Indian focus
        const CURRENCY_LOCALE = 'en-IN';

        // --- FIREBASE SETUP ---
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : null;
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
        
        let app, db, auth;
        let userId = null;
        let balanceState = {
            balance: 0,
            lastCalcDate: null,
            transactions: []
        };

        if (firebaseConfig) {
            app = initializeApp(firebaseConfig);
            db = getFirestore(app);
            auth = getAuth(app);
            // Log for debugging
            // import { setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
            // setLogLevel('Debug');
        } else {
            console.error("Firebase configuration is missing.");
            document.getElementById('content').innerHTML = `
                <div class="mt-16 p-6 bg-red-100 border border-red-400 text-red-700 rounded-xl shadow-lg">
                    <p class="font-bold">Error: Configuration Missing</p>
                    <p>Cannot initialize Firebase. Check console for details.</p>
                </div>`;
        }

        // --- HELPER FUNCTIONS ---

        const formatCurrency = (amount) => {
            return new Intl.NumberFormat(CURRENCY_LOCALE, {
                style: 'currency',
                currency: CURRENCY_CODE,
                minimumFractionDigits: 2,
                maximumFractionDigits: 2,
            }).format(amount);
        };

        const timestampToDate = (timestamp) => {
            return timestamp && timestamp.toDate ? timestamp.toDate() : (timestamp instanceof Date ? timestamp : null);
        };

        const getUserDocRef = (uid) => doc(db, `artifacts/${appId}/users/${uid}/savings/account`);
        const getTransactionsColRef = (uid) => collection(db, `artifacts/${appId}/users/${uid}/savings/transactions`);

        // --- INTEREST CALCULATION LOGIC ---

        const calculateInterest = (currentBalance, lastCalcDate) => {
            let newBalance = currentBalance;
            let newLastCalcDate = lastCalcDate ? new Date(lastCalcDate) : new Date();
            newLastCalcDate.setHours(0, 0, 0, 0); 
            
            let daysPassed = 0;
            let earnedInterest = 0;

            const today = new Date();
            today.setHours(0, 0, 0, 0); 

            let currentDate = new Date(newLastCalcDate);

            // Loop through each full day passed
            while (currentDate < today) {
                currentDate.setDate(currentDate.getDate() + 1);
                
                if (currentDate <= today) {
                    const dailyInterest = newBalance * DAILY_INTEREST_RATE;
                    newBalance += dailyInterest;
                    earnedInterest += dailyInterest;
                    daysPassed++;
                    newLastCalcDate = new Date(currentDate);
                }
            }

            return {
                newBalance: parseFloat(newBalance.toFixed(2)),
                newLastCalcDate,
                daysPassed,
                earnedInterest: parseFloat(earnedInterest.toFixed(2)),
            };
        };

        // --- DATA LISTENERS AND AUTH ---

        const setupDataListeners = (uid) => {
            // 1. Account Balance Listener
            const accountRef = getUserDocRef(uid);
            onSnapshot(accountRef, async (docSnap) => {
                let currentBalance = 0;
                let lastCalcDate = null;

                if (docSnap.exists()) {
                    const data = docSnap.data();
                    currentBalance = data.balance || 0;
                    lastCalcDate = timestampToDate(data.lastInterestCalculation);
                }

                const { newBalance, newLastCalcDate, daysPassed, earnedInterest } = calculateInterest(currentBalance, lastCalcDate);

                if (daysPassed > 0) {
                    // Update Firestore if interest needs to be applied
                    await setDoc(accountRef, {
                        balance: newBalance,
                        lastInterestCalculation: newLastCalcDate,
                        currency: CURRENCY_CODE,
                        interestRate: DAILY_INTEREST_RATE,
                    }, { merge: true });
                    
                    // Add an interest transaction record
                    if (earnedInterest > 0) {
                        await addTransaction(uid, earnedInterest, 'Interest Credited', 'System');
                    }
                }
                
                // Update global state
                balanceState.balance = newBalance;
                balanceState.lastCalcDate = newLastCalcDate;
                renderCurrentView(); // Re-render the current view
                
            }, (error) => {
                console.error("Account Listener Error:", error);
            });

            // 2. Transactions Listener
            const q = query(getTransactionsColRef(uid), orderBy('timestamp', 'desc'), limit(50));
            onSnapshot(q, (snapshot) => {
                balanceState.transactions = snapshot.docs.map(doc => ({
                    id: doc.id,
                    ...doc.data(),
                    date: timestampToDate(doc.data().timestamp)
                }));
                renderCurrentView(); // Re-render the current view
            }, (error) => {
                console.error("Transactions Listener Error:", error);
            });
        };

        // --- TRANSACTION LOGIC ---

        const addTransaction = async (uid, amount, type, source) => {
            if (!uid || amount <= 0) return;
            const transactionsColRef = getTransactionsColRef(uid);
            try {
                await setDoc(doc(transactionsColRef), {
                    amount: amount,
                    type: type, // 'Deposit' or 'Interest Credited'
                    source: source, // e.g., 'Wallet X', 'System'
                    timestamp: serverTimestamp(),
                });
            } catch (e) {
                console.error("Error adding transaction:", e);
            }
        };


        const handleDeposit = async (e) => {
            e.preventDefault();
            if (!userId) return;

            const depositInput = document.getElementById('deposit-amount');
            const walletSelect = document.getElementById('wallet-select');
            const amount = parseFloat(depositInput.value);
            const source = walletSelect.value;
            
            if (isNaN(amount) || amount <= 10) {
                showMessageBox('error', `Deposit amount must be at least ${formatCurrency(10)}.`);
                return;
            }
            
            showLoading(true);

            try {
                const accountRef = getUserDocRef(userId);
                const docSnap = await getDoc(accountRef);
                let currentBalance = docSnap.exists() ? docSnap.data().balance : 0;
                
                // Calculate and apply interest one last time before adding deposit
                const lastCalcDate = docSnap.exists() ? timestampToDate(docSnap.data().lastInterestCalculation) : null;
                const { newBalance: balanceAfterInterest, newLastCalcDate, earnedInterest } = calculateInterest(currentBalance, lastCalcDate);

                const finalBalance = parseFloat((balanceAfterInterest + amount).toFixed(2));

                // 1. Update Account Balance
                await setDoc(accountRef, {
                    balance: finalBalance,
                    lastInterestCalculation: newLastCalcDate,
                    currency: CURRENCY_CODE,
                    interestRate: DAILY_INTEREST_RATE,
                }, { merge: true });

                // 2. Add Deposit Transaction
                await addTransaction(userId, amount, 'Deposit', source);
                
                showMessageBox('success', `${formatCurrency(amount)} deposited successfully from ${source}!`);
                depositInput.value = ''; // Clear input

            } catch (e) {
                console.error("Deposit error:", e);
                showMessageBox('error', "Failed to complete deposit. Please try again.");
            } finally {
                showLoading(false);
                navigateTo('balance'); // Show the updated balance view
            }
        };

        // --- UI RENDERING ---

        let currentView = 'home';
        const contentDiv = document.getElementById('content');
        const navItems = document.querySelectorAll('.nav-item');

        const showLoading = (isLoading) => {
            const loadingSpinner = document.getElementById('loading-spinner');
            if (loadingSpinner) {
                loadingSpinner.classList.toggle('hidden', !isLoading);
            }
        };

        const showMessageBox = (type, message) => {
            const msgBox = document.createElement('div');
            const color = type === 'success' ? 'green' : 'red';
            const icon = type === 'success' ? 'check-circle' : 'alert-triangle';

            msgBox.className = `fixed top-2 left-1/2 transform -translate-x-1/2 p-4 text-sm text-${color}-800 rounded-lg bg-${color}-100 z-50 shadow-2xl flex items-center transition-all duration-300`;
            msgBox.innerHTML = `<i data-lucide="${icon}" class="w-5 h-5 mr-2"></i> ${message}`;
            contentDiv.appendChild(msgBox);
            lucide.createIcons();

            setTimeout(() => {
                msgBox.classList.add('opacity-0', 'scale-90');
                msgBox.addEventListener('transitionend', () => msgBox.remove());
            }, 3000);
        };


        const renderHome = () => {
            contentDiv.innerHTML = `
                <div class="mt-8">
                    <h2 class="text-3xl font-extrabold text-gray-800 mb-2">Welcome Back!</h2>
                    <p class="text-gray-500 mb-8">Secure your future with daily compounding interest.</p>

                    <!-- User Info Card -->
                    <div class="bg-white p-6 rounded-xl shadow-xl border-t-4 border-blue-500 mb-6">
                        <p class="text-sm font-medium text-gray-600 flex items-center">
                            <i data-lucide="user" class="w-4 h-4 mr-2"></i>
                            User ID (Private Key):
                        </p>
                        <p class="font-mono text-xs text-gray-400 break-all mb-4">${userId || 'Authenticating...'}</p>

                        <div class="border-t border-gray-100 pt-4">
                            <p class="text-lg font-bold text-blue-900">${formatCurrency(balanceState.balance)}</p>
                            <p class="text-sm text-gray-500">Current Savings Balance</p>
                        </div>
                    </div>

                    <!-- Options Grid -->
                    <div class="grid grid-cols-2 gap-4">
                        <button data-view="deposit" class="nav-item p-4 bg-white rounded-xl shadow-md text-left transition hover:shadow-lg">
                            <i data-lucide="plus-circle" class="w-6 h-6 text-green-500 mb-2"></i>
                            <h3 class="font-semibold text-gray-700">Add Deposit</h3>
                            <p class="text-xs text-gray-400">Start from ${formatCurrency(10)} daily.</p>
                        </button>
                        <button data-view="balance" class="nav-item p-4 bg-white rounded-xl shadow-md text-left transition hover:shadow-lg">
                            <i data-lucide="wallet" class="w-6 h-6 text-blue-500 mb-2"></i>
                            <h3 class="font-semibold text-gray-700">Check Balance</h3>
                            <p class="text-xs text-gray-400">See interest added.</p>
                        </button>
                        <button data-view="transactions" class="nav-item p-4 bg-white rounded-xl shadow-md text-left transition hover:shadow-lg">
                            <i data-lucide="list-ordered" class="w-6 h-6 text-orange-500 mb-2"></i>
                            <h3 class="font-semibold text-gray-700">See Transactions</h3>
                            <p class="text-xs text-gray-400">View history & interest.</p>
                        </button>
                        <button data-view="deposit" class="nav-item p-4 bg-white rounded-xl shadow-md text-left transition hover:shadow-lg">
                            <i data-lucide="zap" class="w-6 h-6 text-yellow-500 mb-2"></i>
                            <h3 class="font-semibold text-gray-700">Interest Rate</h3>
                            <p class="text-xs text-gray-400">${(DAILY_INTEREST_RATE * 100).toFixed(2)}% Daily</p>
                        </button>
                    </div>

                    <p class="text-center text-xs text-gray-400 mt-8">
                        Data secured with Google Cloud Infrastructure (Firewalls) and Firebase Authentication.
                    </p>
                </div>
            `;
        };
        
        const renderDeposit = () => {
            contentDiv.innerHTML = `
                <div class="mt-8">
                    <h2 class="text-3xl font-extrabold text-blue-800 mb-6">Add Today's Deposit</h2>
                    
                    <form id="deposit-form" class="bg-white p-6 rounded-xl shadow-xl border-t-4 border-green-500">
                        <!-- Wallet Selection (Mocked for Security) -->
                        <div class="mb-6">
                            <label for="wallet-select" class="block text-sm font-medium text-gray-700 mb-2 flex items-center">
                                <i data-lucide="credit-card" class="w-4 h-4 mr-2"></i>
                                Select Digital Wallet (Source)
                            </label>
                            <select id="wallet-select" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-green-500 focus:border-green-500 transition duration-150">
                                <option value="Paytm Wallet">Paytm Wallet</option>
                                <option value="Google Pay (UPI)">Google Pay (UPI)</option>
                                <option value="Debit Card XXXX">Debit Card XXXX</option>
                            </select>
                            <p class="mt-2 text-xs text-red-600 font-semibold flex items-center">
                                <i data-lucide="alert-triangle" class="w-4 h-4 mr-1"></i>
                                WARNING: Real apps NEVER store sensitive wallet details. Data is tokenized by licensed gateways.
                            </p>
                        </div>

                        <!-- Amount Input -->
                        <div class="mb-6">
                            <label for="deposit-amount" class="block text-sm font-medium text-gray-700 mb-2 flex items-center">
                                <i data-lucide="trending-up" class="w-4 h-4 mr-2"></i>
                                Deposit Amount (Minimum ${formatCurrency(10)})
                            </label>
                            <input
                                id="deposit-amount"
                                type="number"
                                step="0.01"
                                min="10"
                                placeholder="${formatCurrency(10.00)} or more..."
                                required
                                class="w-full px-4 py-3 border border-gray-300 rounded-lg text-lg shadow-sm focus:ring-green-500 focus:border-green-500 transition duration-150"
                            />
                        </div>

                        <button
                            type="submit"
                            class="w-full bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-4 rounded-lg shadow-lg transition duration-300 transform hover:scale-[1.01] flex items-center justify-center disabled:opacity-50"
                        >
                            <i data-lucide="dollar-sign" class="w-5 h-5 mr-2"></i>
                            Confirm Deposit & Apply Interest
                        </button>
                    </form>
                </div>
            `;document.getElementById('deposit-form').onsubmit = handleDeposit;
        };

        const renderBalance = () => {
            const today = new Date();
            const lastCalc = balanceState.lastCalcDate ? balanceState.lastCalcDate.toLocaleDateString() : 'N/A';
            
            contentDiv.innerHTML = `
                <div class="mt-8">
                    <h2 class="text-3xl font-extrabold text-blue-800 mb-6">Check Balance & Interest</h2>
                    
                    <!-- Balance Card -->
                    <div class="bg-white p-6 rounded-2xl shadow-2xl border-t-4 border-indigo-500 mb-6">
                        <p class="text-lg font-medium text-gray-600 flex items-center mb-2">
                            <i data-lucide="banknote" class="w-5 h-5 mr-2"></i>
                            Your Total Savings
                        </p>
                        <p class="text-6xl font-black text-indigo-900 truncate">
                            ${formatCurrency(balanceState.balance)}
                        </p>
                        <div class="mt-4 pt-4 border-t border-gray-100 grid grid-cols-2 gap-3">
                            <div>
                                <p class="text-sm text-gray-500 flex items-center">
                                    <i data-lucide="calendar" class="w-4 h-4 mr-1"></i>
                                    Last Interest Date:
                                </p>
                                <p class="text-md font-semibold text-gray-700">${lastCalc}</p>
                            </div>
                            <div>
                                <p class="text-sm text-gray-500 flex items-center">
                                    <i data-lucide="zap" class="w-4 h-4 mr-1"></i>
                                    Daily Interest Rate:
                                </p>
                                <p class="text-md font-semibold text-gray-700">${(DAILY_INTEREST_RATE * 100).toFixed(2)}%</p>
                            </div>
                        </div>
                    </div>

                    <p class="text-center text-sm text-gray-500 mt-6">
                        Your balance is automatically updated every time you load the app or make a deposit to ensure daily interest compounding.
                    </p>
                </div>
            `;
        };
        
        const renderTransactions = () => {
            const history = balanceState.transactions;
            
            contentDiv.innerHTML = `
                <div class="mt-8">
                    <h2 class="text-3xl font-extrabold text-blue-800 mb-6">Transaction History</h2>
                    
                    <div class="bg-white p-4 rounded-xl shadow-xl">
                        ${history.length === 0 ? 
                            `<div class="text-center py-8 text-gray-500">
                                <i data-lucide="inbox" class="w-10 h-10 mx-auto mb-2 text-gray-400"></i>
                                <p>No transactions found yet. Make a deposit!</p>
                            </div>` :
                            `<ul class="divide-y divide-gray-100">
                                ${history.map(tx => `
                                  <li class="py-3 flex justify-between items-center">
                                  <div class="flex items-center">
                                  <div class="p-2 mr-3 rounded-full ${tx.type === 'Deposit' ? 'bg-green-100 text-green-600' : 'bg-yellow-100 text-yellow-600'}">
                                    <i data-lucide="${tx.type === 'Deposit' ? 'arrow-down-left' : 'dollar-sign'}" class="w-4 h-4"></i>
                                   </div>
                                  <div>
                                  <p class="font-semibold text-gray-800">${tx.type}</p>
                                  <p class="text-xs text-gray-500">${tx.source} (${tx.date ? tx.date.toLocaleDateString() : 'Loading...'})</p>
                                  </div>                     
                                  </div>
                                    <p class="font-bold ${tx.type === 'Deposit' ? 'text-green-600' : 'text-yellow-600'}">                                         ${formatCurrency(tx.amount)}        </p>                               </li>                             `).join('')}
   </ul>`     }
      </div>
<p class="text-center text-xs text-gray-400 mt-6">Showing last ${history.length} transactions.</p>
                </div>
            `;
        };
        
const renderCurrentView = () => {          
            lucide.createIcons();
            
switch (currentView) {
                case 'deposit':
                    renderDeposit();
                    break;
                case 'balance':
                    renderBalance();
         break;
          case 'transactions':
                    renderTransactions();
         break;
          case 'home':
          default:
renderHome();
      break;
            }
lucide.createIcons();
        };

const navigateTo = (view) => {
currentView = view;                        
navItems.forEach(item => {
                item.classList.remove('active-tab', 'text-blue-700');
                item.classList.add('text-gray-500');
                if (item.getAttribute('data-view') === view) {
                    item.classList.add('active-tab', 'text-blue-700');
                    item.classList.remove('text-gray-500');
                }
            });
            
renderCurrentView();                 document.getElementById('content').scrollTo({ top: 0, behavior: 'smooth' });
        };


        // --- INITIALIZATION ---
const initApp = async () => {
            if (!db || !auth) {
                showLoading(false);
                return;
            }                                    onAuthStateChanged(auth, async (user) => {
                if (!user) {
                    try {
                        if (initialAuthToken) {
await signInWithCustomToken(auth, initialAuthToken);
                        } else {
await signInAnonymously(auth);
                        }
                    } catch (e) {
                        console.error("Auth failed:", e);
                        showMessageBox('error', 'Authentication failed. Data cannot be loaded.');
                        showLoading(false);
                        return;
                    }
                }          
userId = auth.currentUser.uid;
                setupDataListeners(userId);
showLoading(false);
navigateTo('home');
            });
        };  document.addEventListener('DOMContentLoaded', () => {
navItems.forEach(item => {
                item.addEventListener('click', (e) => {
const view = e.currentTarget.getAttribute('data-view');
                    navigateTo(view);
                });
                // Also handle the home buttons on the home screen
                item.addEventListener('click', (e) => {
                    if (e.target.closest('.nav-item')) {
const view = e.target.closest('.nav-item').getAttribute('data-view');
                        if (view)
                        navigateTo(view);
                    }
                });
            });
            
            initApp();
        });
  </script>
</body>
</html>
