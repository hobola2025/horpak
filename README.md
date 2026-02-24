<!DOCTYPE html>
<html lang="lo">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Dorm Tracker Mobile Pro</title>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+Lao:wght@400;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        :root { --main: #4f46e5; --success: #10b981; --danger: #ef4444; --bg: #f3f4f6; --excel: #059669; --edit: #f59e0b; }
        body { font-family: 'Noto Sans Lao', sans-serif; background-color: var(--bg); margin: 0; padding: 0; }
        .mobile-app { width: 100%; max-width: 500px; min-height: 100vh; background: #ffffff; margin: auto; display: flex; flex-direction: column; }
        
        header { background: var(--main); color: white; padding: 25px 20px; text-align: center; border-radius: 0 0 25px 25px; }

        .stats-bar { display: flex; justify-content: space-around; padding: 15px; background: white; margin: -25px 20px 20px 20px; border-radius: 15px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
        .stat-item h4 { margin: 0; font-size: 11px; color: #6b7280; }
        .stat-item p { margin: 0; font-size: 18px; font-weight: bold; color: var(--main); }

        .form-box { padding: 0 20px; display: flex; flex-direction: column; gap: 10px; }
        .photo-upload { display: flex; align-items: center; gap: 12px; background: #f9fafb; padding: 10px; border-radius: 12px; border: 1px dashed #d1d5db; cursor: pointer; }
        .photo-preview { width: 50px; height: 50px; border-radius: 50%; object-fit: cover; background: #e5e7eb; border: 2px solid white; }
        
        input, select { padding: 12px; border: 1px solid #d1d5db; border-radius: 10px; font-size: 15px; outline: none; transition: 0.3s; background: white; }
        input:focus, select:focus { border-color: var(--main); }
        .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        
        .btn-add { background: var(--main); color: white; border: none; padding: 14px; border-radius: 12px; font-weight: bold; font-size: 16px; margin-top: 5px; cursor: pointer; }
        .btn-update { background: var(--edit); color: white; border: none; padding: 14px; border-radius: 12px; font-weight: bold; font-size: 16px; margin-top: 5px; cursor: pointer; display: none; }
        .btn-excel { background: var(--excel); color: white; border: none; padding: 10px; border-radius: 10px; margin: 0 20px 15px; font-weight: bold; cursor: pointer; display: flex; align-items: center; justify-content: center; gap: 8px; }

        .search-container { padding: 0 20px; margin-bottom: 10px; position: relative; }
        .search-input { width: 100%; box-sizing: border-box; padding: 12px 40px 12px 15px; border-radius: 25px; border: 1px solid #e5e7eb; background: #f9fafb; font-size: 14px; }
        .search-icon { position: absolute; right: 35px; top: 50%; transform: translateY(-50%); color: #9ca3af; }

        .list-container { flex-grow: 1; padding: 10px 20px; overflow-y: auto; }
        .student-card { background: white; border: 1px solid #f3f4f6; padding: 12px; border-radius: 15px; margin-bottom: 12px; display: flex; align-items: center; gap: 12px; box-shadow: 0 2px 5px rgba(0,0,0,0.03); position: relative; }
        
        .img-area { position: relative; }
        .student-no { position: absolute; top: -5px; left: -5px; background: var(--main); color: white; font-size: 10px; width: 20px; height: 20px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: bold; border: 2px solid white; }
        .student-img { width: 65px; height: 65px; border-radius: 50%; object-fit: cover; border: 2px solid #f3f4f6; }
        
        .info { flex-grow: 1; }
        .info b { font-size: 15px; color: #111827; display: block; margin-bottom: 2px; }
        .info span { font-size: 11px; color: #6b7280; display: block; line-height: 1.5; }
        .info i { color: var(--main); width: 16px; text-align: center; margin-right: 4px; }
        
        .role-badge { display: inline-block; padding: 2px 8px; border-radius: 6px; font-size: 10px; font-weight: bold; margin-bottom: 4px; }
        .role-teacher { background: #fef3c7; color: #92400e; }
        .role-board { background: #dbeafe; color: #1e40af; }
        .role-member { background: #f3f4f6; color: #374151; }

        .actions { display: flex; flex-direction: column; gap: 8px; align-items: flex-end; }
        .status-btn { padding: 6px 10px; border-radius: 20px; border: none; font-weight: bold; font-size: 11px; min-width: 75px; cursor: pointer; }
        .in { background: #dcfce7; color: #059669; }
        .out { background: #fee2e2; color: #dc2626; }
        
        .tool-btns { display: flex; gap: 10px; }
        .edit-btn { color: var(--edit); border: none; background: none; font-size: 16px; cursor: pointer; }
        .del-btn { color: #d1d5db; border: none; background: none; font-size: 16px; cursor: pointer; }
    </style>
</head>
<body>

<div class="mobile-app">
    <header>
        <h2 style="margin:0">ຕິດຕາມນັກສຶກສານອນຫໍພັກໃນ</h2>
		 <h3 style="margin:0">ໂຮງຮຽນສາທາລະນະສຸກອຸດົມໄຊ</h3>
		<br>
       
    </header>

    <div class="stats-bar">
        <div class="stat-item"><h4>ທັງໝົດ</h4><p id="total">0</p></div>
        <div class="stat-item"><h4>ຢູ່ຫໍ</h4><p id="in">0</p></div>
        <div class="stat-item"><h4>ອອກນອກ</h4><p id="out">0</p></div>
    </div>

    <div class="form-box" id="studentForm">
        <input type="hidden" id="editId">
        <label class="photo-upload">
            <img id="imgPreview" class="photo-preview" src="https://cdn-icons-png.flaticon.com/512/149/149071.png">
            <span style="font-size: 12px; color: #6b7280;">ກົດເລືອກຮູບປະຈຳຕົວ</span>
            <input type="file" id="fileInp" accept="image/*" style="display:none" onchange="previewImage(this)">
        </label>

        <input type="text" id="name" placeholder="ຊື່ ແລະ ນາມສະກຸນ">
        
        <div class="grid-2">
            <select id="role">
                <option>ເລືອກພະລະບົດບາດເຮົາ...</option>
				<option value="ສະມາຊິກ">ຄູປະຈຳຫໍພັກ</option>
                <option value="ຄະນະຫໍ">ຄະນະຫໍພັກ</option>
                <option value="ຄູປະຈຳຫໍພັກ">ສະມາຊິກຫໍພັກ</option>
            </select>
            <input type="text" id="grade" placeholder="ຫຼັກສູດ/ຊັ້ນຮຽນ">
        </div>

        <div class="grid-2">
            <input type="text" id="room" placeholder="ຫ້ອງນອນ">
            <input type="text" id="plate" placeholder="ປ້າຍລົດຖ້າມີ">
        </div>

        <input type="tel" id="phone" placeholder="ເບີໂທລະສັບ">

        <button id="addBtn" class="btn-add" onclick="addStudent()">ເພີ່ມນັກສາເຂົ້າໃໝ່</button>
        <button id="updateBtn" class="btn-update" onclick="saveEdit()">ບັນທຶກການແກ້ໄຂ</button>
    </div>

    <hr style="margin: 15px 20px; border: 0.5px solid #eee;">
    
    <div class="search-container">
        <i class="fas fa-search search-icon"></i>
        <input type="text" id="searchInput" class="search-input" placeholder="ຄົ້ນຫາ: ຊື່, ຫ້ອງ, ຊັ້ນຮຽນ, ປ້າຍລົດ..." onkeyup="render()">
    </div>

    <button class="btn-excel" onclick="exportToExcel()">
        <i class="fas fa-file-excel"></i> ດາວໂຫລດຂໍ້ມູນທັງໝົດເປັນ Excel
    </button>

    <div class="list-container" id="studentList"></div>
</div>

<script>
    let students = JSON.parse(localStorage.getItem('dorm_v4_data')) || [];
    let currentBase64 = "https://cdn-icons-png.flaticon.com/512/149/149071.png";

    function previewImage(input) {
        if (input.files && input.files[0]) {
            let reader = new FileReader();
            reader.onload = (e) => {
                currentBase64 = e.target.result;
                document.getElementById('imgPreview').src = currentBase64;
            };
            reader.readAsDataURL(input.files[0]);
        }
    }

    function addStudent() {
        const name = document.getElementById('name').value;
        const role = document.getElementById('role').value;
        const grade = document.getElementById('grade').value;
        const room = document.getElementById('room').value;
        const phone = document.getElementById('phone').value;
        const plate = document.getElementById('plate').value || '-';

        if(name && grade && room && phone) {
            // ເພີ່ມຂໍ້ມູນເຂົ້າ array (ໃສ່ status ເປັນ 'in' ເລີຍ)
            students.push({ 
                id: Date.now(), 
                name, role, grade, room, phone, plate, 
                photo: currentBase64, 
                status: 'in' 
            });
            clearForm();
            updateApp();
        } else { alert("ກະລຸນາປ້ອນຂໍ້ມູນໃຫ້ຄົບ"); }
    }

    function editStudent(id) {
        const s = students.find(item => item.id === id);
        if(s) {
            document.getElementById('editId').value = s.id;
            document.getElementById('name').value = s.name;
            document.getElementById('role').value = s.role;
            document.getElementById('grade').value = s.grade;
            document.getElementById('room').value = s.room;
            document.getElementById('phone').value = s.phone;
            document.getElementById('plate').value = s.plate;
            document.getElementById('imgPreview').src = s.photo;
            currentBase64 = s.photo;

            document.getElementById('addBtn').style.display = 'none';
            document.getElementById('updateBtn').style.display = 'block';
            window.scrollTo({top: 0, behavior: 'smooth'});
        }
    }

    function saveEdit() {
        const id = parseInt(document.getElementById('editId').value);
        const index = students.findIndex(s => s.id === id);
        
        if(index !== -1) {
            students[index].name = document.getElementById('name').value;
            students[index].role = document.getElementById('role').value;
            students[index].grade = document.getElementById('grade').value;
            students[index].room = document.getElementById('room').value;
            students[index].phone = document.getElementById('phone').value;
            students[index].plate = document.getElementById('plate').value;
            students[index].photo = currentBase64;

            clearForm();
            updateApp();
            alert("ແກ້ໄຂຂໍ້ມູນສຳເລັດ");
        }
    }

    function clearForm() {
        document.getElementById('editId').value = '';
        document.getElementById('name').value = '';
        document.getElementById('grade').value = '';
        document.getElementById('room').value = '';
        document.getElementById('phone').value = '';
        document.getElementById('plate').value = '';
        document.getElementById('role').value = 'ສະມາຊິກ';
        document.getElementById('imgPreview').src = "https://cdn-icons-png.flaticon.com/512/149/149071.png";
        currentBase64 = "https://cdn-icons-png.flaticon.com/512/149/149071.png";
        
        document.getElementById('addBtn').style.display = 'block';
        document.getElementById('updateBtn').style.display = 'none';
    }

    function updateApp() {
        localStorage.setItem('dorm_v4_data', JSON.stringify(students));
        render();
    }

    function toggleStatus(id) {
        students = students.map(s => {
            if(s.id === id) s.status = (s.status === 'in' ? 'out' : 'in');
            return s;
        });
        updateApp();
    }

    function deleteStudent(id) {
        if(confirm("ຢືນຢັນການລຶບລາຍຊື່?")) {
            students = students.filter(s => s.id !== id);
            updateApp();
        }
    }

    function exportToExcel() {
        if(students.length === 0) return alert("ບໍ່ມີຂໍ້ມູນ");
        let csv = "\uFEFF"; 
        csv += "ລຳດັບ,ບົດບາດ,ຊື່,ຊັ້ນຮຽນ,ຫ້ອງນອນ,ເບີໂທ,ປ້າຍລົດ,ສະຖານະ\n";
        students.forEach((s, i) => {
            csv += `${i+1},${s.role},${s.name},${s.grade},${s.room},${s.phone},${s.plate},${s.status=='in'?'ຢູ່ຫໍ':'ອອກນອກ'}\n`;
        });
        const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url;
        a.download = `dorm_report_${new Date().toLocaleDateString()}.csv`;
        a.click();
    }

    function render() {
        const container = document.getElementById('studentList');
        const searchTerm = document.getElementById('searchInput').value.toLowerCase();
        container.innerHTML = '';
        
        let inCount = 0;
        
        // ຄົ້ນຫາຂໍ້ມູນ
        const filteredStudents = students.filter(s => 
            s.name.toLowerCase().includes(searchTerm) || 
            s.room.toLowerCase().includes(searchTerm) || 
            s.grade.toLowerCase().includes(searchTerm) || 
            s.plate.toLowerCase().includes(searchTerm)
        );

        students.forEach(s => { if(s.status === 'in') inCount++; });
        document.getElementById('total').innerText = students.length;
        document.getElementById('in').innerText = inCount;
        document.getElementById('out').innerText = students.length - inCount;

        filteredStudents.forEach((s, index) => {
            let roleClass = 'role-member';
            if(s.role === 'ຄູປະຈຳຫໍພັກ') roleClass = 'role-teacher';
            if(s.role === 'ຄະນະຫໍ') roleClass = 'role-board';

            // ຊອກຫາລຳດັບທີ່ແທ້ຈິງໃນ Array ຫຼັກ (1,2,3...)
            const realIndex = students.findIndex(item => item.id === s.id) + 1;

            container.innerHTML += `
                <div class="student-card">
                    <div class="img-area">
                        <div class="student-no">${realIndex}</div>
                        <img src="${s.photo}" class="student-img">
                    </div>
                    <div class="info">
                        <span class="role-badge ${roleClass}">${s.role}</span>
                        <b>${s.name}</b>
                        <span><i class="fas fa-graduation-cap"></i> ${s.grade}</span>
                        <span><i class="fas fa-bed"></i> ຫ້ອງ: ${s.room} | <i class="fas fa-motorcycle"></i> ${s.plate}</span>
                        <span><i class="fas fa-phone"></i> ${s.phone}</span>
                    </div>
                    <div class="actions">
                        <button class="status-btn ${s.status}" onclick="toggleStatus(${s.id})">
                            ${s.status === 'in' ? 'ຢູ່ຫໍ' : 'ອອກນອກ'}
                        </button>
                        <div class="tool-btns">
                             <button class="edit-btn" onclick="editStudent(${s.id})">
                                <i class="fas fa-edit"></i>
                            </button>
                            <button class="del-btn" onclick="deleteStudent(${s.id})">
                                <i class="fas fa-trash"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `;
        });
    }

    render();
</script>

</body>
</html>
