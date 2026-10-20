# Macluumaad-shakhsiyadka-muhiimka-ah
Any time
<!DOCTYPE html>
<html lang="so">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>InBio Contact Manager</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        body { background-color: #f3f4f6; color: #3d424a; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        .inbio-card { background: #ffffff; border-radius: 16px; box-shadow: 10px 10px 19px #1c1e220a, -10px -10px 19px #ffffff; }
        .inbio-btn-primary { background: linear-gradient(145deg, #ff014f, #d1003e); color: white; box-shadow: 4px 4px 15px rgba(255, 1, 79, 0.3); }
        .inbio-btn-primary:hover { background: linear-gradient(145deg, #d1003e, #ff014f); }
    </style>
</head>
<body class="min-h-screen pb-12">

    <!-- Login Modal -->
    <div id="loginModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 flex items-center justify-center p-4">
        <div class="inbio-card p-8 max-w-md w-full text-center">
            <div class="w-16 h-16 bg-rose-100 text-[#ff014f] rounded-full flex items-center justify-center mx-auto mb-4 text-2xl font-bold">
                <i class="fas fa-lock"></i>
            </div>
            <h2 class="text-2xl font-bold text-gray-800 mb-2">Geli Password-ka</h2>
            <p class="text-gray-500 mb-6 text-sm">Geli password-ka si aad u farto Dashboord-ka</p>
            <input type="password" id="loginPassword" placeholder="Password-kaaga..." class="w-full px-4 py-3 rounded-lg border border-gray-200 mb-4 focus:outline-none focus:border-[#ff014f]">
            <p id="loginError" class="text-rose-500 text-sm mb-4 hidden">Password-ku waa makhalad!</p>
            <button onclick="checkPassword()" class="inbio-btn-primary w-full py-3 rounded-lg font-semibold transition">Fur Dashboord-ka</button>
        </div>
    </div>

    <!-- Main App Container -->
    <div id="appContainer" class="hidden max-w-6xl mx-auto px-4 pt-8">
        <!-- Header -->
        <header class="flex flex-col md:flex-row justify-between items-center mb-8 gap-4">
            <div>
                <h1 class="text-3xl font-extrabold text-gray-800">Kaydka Xogta Dadka</h1>
                <p class="text-gray-500 text-sm">Maamul oo kayso xogta dadka aad isbarataan</p>
            </div>
            <div class="flex gap-3">
                <button onclick="openModal()" class="inbio-btn-primary px-5 py-2.5 rounded-lg font-medium flex items-center gap-2">
                    <i class="fas fa-plus"></i> Diwaan-geli Qof Cusub
                </button>
                <button onclick="changePasswordPrompt()" class="bg-white border border-gray-200 text-gray-700 px-4 py-2.5 rounded-lg font-medium hover:bg-gray-50 flex items-center gap-2">
                    <i class="fas fa-[#ff014f] fa-key"></i> Password-ka
                </button>
            </div>
        </header>

        <!-- Search and Filter Bar -->
        <div class="inbio-card p-4 mb-8 flex flex-col md:flex-row gap-4 items-center justify-between">
            <div class="relative w-full md:w-1/2">
                <i class="fas fa-search absolute left-4 top-3.5 text-gray-400"></i>
                <input type="text" id="searchInput" oninput="renderContacts()" placeholder="Raadi Magac, Tel, Email, ama Shaqo..." class="w-full pl-11 pr-4 py-2.5 border border-gray-200 rounded-lg focus:outline-none focus:border-[#ff014f]">
            </div>
            <div class="flex gap-3 w-full md:w-auto">
                <select id="filterGender" onchange="renderContacts()" class="px-3 py-2.5 border border-gray-200 rounded-lg bg-white text-sm focus:outline-none">
                    <option value="">Dhammaan Jinsiga</option>
                    <option value="Rag">Rag</option>
                    <option value="Dumar">Dumar</option>
                </select>
                <select id="filterMarital" onchange="renderContacts()" class="px-3 py-2.5 border border-gray-200 rounded-lg bg-white text-sm focus:outline-none">
                    <option value="">Xaaladda Qoyska</option>
                    <option value="Doob">Doob</option>
                    <option value="Xasle">Xasle/Guri-leh</option>
                </select>
            </div>
        </div>

        <!-- Contacts Grid -->
        <div id="contactsGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <!-- Dynamic contact cards will go here -->
        </div>
    </div>

    <!-- Add/Edit Modal -->
    <div id="contactModal" class="fixed inset-0 bg-black/50 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="inbio-card p-6 max-w-xl w-full max-h-[90vh] overflow-y-auto">
            <div class="flex justify-between items-center mb-6">
                <h3 id="modalTitle" class="text-xl font-bold text-gray-800">Diwaan-geli Qof Cusub</h3>
                <button onclick="closeModal()" class="text-gray-400 hover:text-gray-600"><i class="fas fa-times text-xl"></i></button>
            </div>
            <form id="contactForm" onsubmit="saveContact(event)" class="space-y-4">
                <input type="hidden" id="contactId">
                <div>
                    <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Sawirka Qofka</label>
                    <input type="file" id="photoInput" accept="image/*" class="w-full text-sm text-gray-500 file:mr-4 file:py-2 file:px-4 file:rounded-lg file:border-0 file:text-sm file:font-semibold file:bg-rose-50 file:text-[#ff014f]">
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Magaca Buuxa *</label>
                        <input type="text" id="nameInput" required class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-[#ff014f]">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Lambaarka *</label>
                        <input type="text" id="phoneInput" required class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-[#ff014f]">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Email-ka</label>
                        <input type="email" id="emailInput" class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-[#ff014f]">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Shaqada/Booska</label>
                        <input type="text" id="jobInput" class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-[#ff014f]">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Da'da</label>
                        <input type="number" id="ageInput" class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-[#ff014f]">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Jinsiga</label>
                        <select id="genderInput" class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-[#ff014f]">
                            <option value="Rag">Rag</option>
                            <option value="Dumar">Dumar</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Xaalada Qoyska</label>
                        <select id="maritalInput" class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-[#ff014f]">
                            <option value="Doob">Doob</option>
                            <option value="Xasle">Xasle/Guri-leh</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Halka uu degan yahay</label>
                        <input type="text" id="locationInput" class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-[#ff014f]">
                    </div>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-gray-600 uppercase mb-1">Faahfaahin Dheeraad ah</label>
                    <textarea id="notesInput" rows="3" class="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-[#ff014f]"></textarea>
                </div>
                <div class="flex justify-end gap-3 pt-4">
                    <button type="button" onclick="closeModal()" class="px-4 py-2 border rounded-lg text-gray-600 hover:bg-gray-50">Ka noqod</button>
                    <button type="submit" class="inbio-btn-primary px-6 py-2 rounded-lg font-semibold">Kaydi</button>
                </div>
            </form>
        </div>
    </div>

    <script>
        let appPassword = localStorage.getItem('inbio_pwd') || 'Password123!';
        let contacts = JSON.parse(localStorage.getItem('inbio_contacts')) || [];

        function checkPassword() {
            const pwd = document.getElementById('loginPassword').value;
            if(pwd === appPassword) {
                document.getElementById('loginModal').classList.add('hidden');
                document.getElementById('appContainer').classList.remove('hidden');
                renderContacts();
            } else {
                document.getElementById('loginError').classList.remove('hidden');
            }
        }

        function changePasswordPrompt() {
            const newPwd = prompt("Geli Password-ka cusub:");
            if(newPwd && newPwd.trim() !== "") {
                appPassword = newPwd.trim();
                localStorage.setItem('inbio_pwd', appPassword);
                alert("Password-ka waa la bedelay!");
            }
        }

        function renderContacts() {
            const grid = document.getElementById('contactsGrid');
            const search = document.getElementById('searchInput').value.toLowerCase();
            const gFilter = document.getElementById('filterGender').value;
            const mFilter = document.getElementById('filterMarital').value;

            grid.innerHTML = '';

            const filtered = contacts.filter(c => {
                const matchSearch = c.name.toLowerCase().includes(search) || 
                                    c.phone.includes(search) || 
                                    (c.email && c.email.toLowerCase().includes(search)) ||
                                    (c.job && c.job.toLowerCase().includes(search));
                const matchGender = !gFilter || c.gender === gFilter;
                const matchMarital = !mFilter || c.marital === mFilter;
                return matchSearch && matchGender && matchMarital;
            });

            filtered.forEach(c => {
                grid.innerHTML += `
                    <div class="inbio-card p-5 relative flex flex-col justify-between">
                        <div>
                            <div class="flex gap-4 items-center mb-4">
                                <img src="${c.photo || 'https://via.placeholder.com/80'}" class="w-16 h-16 rounded-full object-cover border-2 border-rose-500">
                                <div>
                                    <h4 class="font-bold text-gray-800 text-lg">${c.name}</h4>
                                    <p class="text-sm text-[#ff014f] font-medium">${c.job || 'Aan la cayimin'}</p>
                                </div>
                            </div>
                            <div class="space-y-2 text-sm text-gray-600 mb-4">
                                <p><i class="fas fa-phone w-5 text-gray-400"></i> ${c.phone}</p>
                                ${c.email ? `<p><i class="fas fa-envelope w-5 text-gray-400"></i> ${c.email}</p>` : ''}
                                <p><i class="fas fa-user w-5 text-gray-400"></i> ${c.gender || '-'} | ${c.age ? c.age + ' Sano' : '-'} | ${c.marital || '-'}</p>
                                <p><i class="fas fa-map-marker-alt w-5 text-gray-400"></i> ${c.location || 'Aan la cayimin'}</p>
                                ${c.notes ? `<p class="text-xs bg-gray-50 p-2 rounded border mt-2">${c.notes}</p>` : ''}
                            </div>
                        </div>
                        <div class="flex justify-between items-center border-t pt-3 mt-2">
                            <div class="flex gap-2">
                                <a href="tel:${c.phone}" class="w-8 h-8 rounded-full bg-green-100 text-green-600 flex items-center justify-center"><i class="fas fa-phone text-xs"></i></a>
                                <a href="https://wa.me/${c.phone.replace(/[^0-9]/g, '')}" target="_blank" class="w-8 h-8 rounded-full bg-emerald-100 text-emerald-600 flex items-center justify-center"><i class="fab fa-whatsapp text-xs"></i></a>
                            </div>
                            <div class="flex gap-2">
                                <button onclick="editContact('${c.id}')" class="text-gray-400 hover:text-blue-600"><i class="fas fa-edit"></i></button>
                                <button onclick="deleteContact('${c.id}')" class="text-gray-400 hover:text-rose-600"><i class="fas fa-trash"></i></button>
                            </div>
                        </div>
                    </div>
                `;
            });
        }

        function openModal() {
            document.getElementById('contactForm').reset();
            document.getElementById('contactId').value = '';
            document.getElementById('modalTitle').innerText = 'Diwaan-geli Qof Cusub';
            document.getElementById('contactModal').classList.remove('hidden');
        }

        function closeModal() {
            document.getElementById('contactModal').classList.add('hidden');
        }

        function saveContact(e) {
            e.preventDefault();
            const id = document.getElementById('contactId').value || Date.now().toString();
            const photoFile = document.getElementById('photoInput').files[0];

            const contactData = {
                id,
                name: document.getElementById('nameInput').value,
                phone: document.getElementById('phoneInput').value,
                email: document.getElementById('emailInput').value,
                job: document.getElementById('jobInput').value,
                age: document.getElementById('ageInput').value,
                gender: document.getElementById('genderInput').value,
                marital: document.getElementById('maritalInput').value,
                location: document.getElementById('locationInput').value,
                notes: document.getElementById('notesInput').value,
                photo: ''
            };

            const existing = contacts.find(c => c.id === id);
            if(existing) contactData.photo = existing.photo;

            if(photoFile) {
                const reader = new FileReader();
                reader.onloadend = function() {
                    contactData.photo = reader.result;
                    commitSave(contactData);
                }
                reader.readAsDataURL(photoFile);
            } else {
                commitSave(contactData);
            }
        }

        function commitSave(data) {
            const index = contacts.findIndex(c => c.id === data.id);
            if(index > -1) contacts[index] = data;
            else contacts.unshift(data);

            localStorage.setItem('inbio_contacts', JSON.stringify(contacts));
            closeModal();
            renderContacts();
        }

        function editContact(id) {
            const c = contacts.find(item => item.id === id);
            if(!c) return;

            document.getElementById('contactId').value = c.id;
            document.getElementById('nameInput').value = c.name;
            document.getElementById('phoneInput').value = c.phone;
            document.getElementById('emailInput').value = c.email || '';
            document.getElementById('jobInput').value = c.job || '';
            document.getElementById('ageInput').value = c.age || '';
            document.getElementById('genderInput').value = c.gender || 'Rag';
            document.getElementById('maritalInput').value = c.marital || 'Doob';
            document.getElementById('locationInput').value = c.location || '';
            document.getElementById('notesInput').value = c.notes || '';

            document.getElementById('modalTitle').innerText = 'Bedel Xogta Qofka';
            document.getElementById('contactModal').classList.remove('hidden');
        }

        function deleteContact(id) {
            if(confirm("Ma ziidocaa in aad tirto qofkan?")) {
                contacts = contacts.filter(c => c.id !== id);
                localStorage.setItem('inbio_contacts', JSON.stringify(contacts));
                renderContacts();
            }
        }
    </script>
</body>
</html>
