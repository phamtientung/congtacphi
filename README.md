<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>Quản lý chi tiêu & Giấy đề nghị thanh toán</title>
  <style>
@media print {
    .btn, #phieuArea .btn {
    display: none !important;
  }
}
 @page {
    size: A4;
    margin: 15mm;
}

#phieuArea .header {
  position: relative;
  margin-bottom: 10px;
}

#phieuArea .logo {
  position: absolute;
  top: 0;
  left: 0;
}

#phieuArea .logo img {
  width: 120px;  /* chỉnh kích thước logo */
  height: auto;
}

#phieuArea .title {
  text-align: center;
}


    body { font-family: "Times New Roman", serif; margin: 40px; -webkit-print-color-adjust: exact;
    print-color-adjust: exact; }
    .container { width: 850px; margin: auto; border: 1px solid #000; padding: 40px; font-size: 20px; }
    .hidden { display: none; }
    h2 { text-align: center; margin-bottom: 15px; }
    .btn { margin: 3px; padding: 8px 16px; background: #0066cc; color: #fff; border: none; border-radius: 5px; cursor: pointer; font-size: 16px; }
    .btn.gray { background: gray; }
    .btn.green { background: green; }
    .btn.orange { background: orange; }
    table { width: 100%; border-collapse: collapse; margin-top: 15px; font-size: 16px; }
    table, th, td { border: 1px solid #000; text-align: center; }
    th, td { padding: 6px; text-align: center;width: 250px;border: 1px solid #000; }
    .right { text-align: center; }
    .delBtn { background: red; color: white; border: none; padding: 3px 8px; cursor: pointer; border-radius: 3px; }
    input, select { padding: 6px; margin: 3px; font-size: 16px; }
    .row { display: flex; align-items: center; margin-bottom: 8px; flex-wrap: wrap; }
    .row label { min-width: 160px; }
    .selected-cb { display: inline-block; background: #f0f0f0; padding: 5px 10px; border-radius: 12px; margin: 2px; }
    .selected-cb span { color: red; margin-left: 5px; cursor: pointer; }
    .sign { display: flex; justify-content: space-between; margin-top: 10px; }
    .sign div { width: 30%; text-align: center; min-height: 80px; }
    /* Edit panel */
    #editArea { width: 500px; margin: 15px auto; border: 1px dashed #666; padding: 12px; background: #fafafa; }
    #editArea table, #editArea th, #editArea td { border: 1px solid #ccc; text-align: left; }
    #editArea th, #editArea td { padding: 8px; }
    td.editable { cursor: pointer; background: #fff; }
    td.editable input { width: 100%; box-sizing: border-box; font-size: 16px; padding: 4px; }
    /* date row above sign, distributed in 3 columns to place date above right-most (Thủ trưởng) */
    .dateRow { display:flex; justify-content:space-between; margin-top:8px; margin-bottom:6px; }
    .dateRowSingle {
  text-align: right;
  margin-top: 8px;
  margin-bottom: 6px;
  font-style: italic;

}
    .dateRow div { width:30%; text-align:center; }
    .dateRight { text-align:right; }
  </style>
</head>
<body>

  <!-- ===== Đăng nhập ===== -->
  <div id="loginArea" class="container">
    <h2>Đăng nhập</h2>
    <p><input id="username" placeholder="Tài khoản" style="width:100%"></p>
    <p><input id="password" type="password" placeholder="Mật khẩu" style="width:100%"></p>
    <button class="btn" onclick="login()">Đăng nhập</button>
    <button class="btn green" onclick="showRegister()">Đăng ký</button>
    <button class="btn orange" onclick="showManage()">Quản lý TK</button>
    <p id="loginMsg" style="color:red"></p>
  </div>

  <!-- ===== Đăng ký ===== -->
  <div id="registerArea" class="container hidden">
    <h2>Đăng ký tài khoản</h2>
    <p><input id="regHoten" placeholder="Họ tên" style="width:100%"></p>
    <p><input id="regUser" placeholder="Tên đăng nhập" style="width:100%"></p>
    <p><input id="regPass" type="password" placeholder="Mật khẩu" style="width:100%"></p>
    <p><input id="regTieuvat" type="number" placeholder="Mức tiêu vặt/ngày (VNĐ)" style="width:100%"></p>
    <button class="btn" onclick="register()">Tạo tài khoản</button>
    <button class="btn gray" onclick="backLogin()">Quay lại</button>
    <p id="regMsg" style="color:red"></p>
  </div>

  <!-- ===== Quản lý tài khoản ===== -->
  <div id="manageArea" class="container hidden">
    <h2>Quản lý tài khoản</h2>
    <table id="userTable">
      <thead>
        <tr>
          <th>STT</th>
          <th>Họ tên</th>
          <th>Tài khoản</th>
          <th>Tiêu vặt/ngày</th>
          <th>Hành động</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
    <button class="btn gray" onclick="backLogin()">Quay lại</button>
  </div>

  <!-- Edit panel (ẩn) -->
  <div id="editArea" class="hidden">
    <h3>Sửa tài khoản</h3>
    <table id="editTable" style="width:100%;"></table>
    <div style="text-align:right; margin-top:10px;">
      <button class="btn gray" onclick="cancelEdit()">Hủy</button>
      <button class="btn green" onclick="saveEditPanel()">Lưu</button>
    </div>
  </div>

  <!-- ===== Nhập chi tiêu ===== -->
  <div id="inputArea" class="container hidden">
  <h2>Nhập chi tiêu</h2>
  <p>Xin chào, <b id="userName"></b>!</p>
  

    <div class="row">
      <label>Cán bộ công tác:</label>
      <select id="cbSelect"><option value="">-- Chọn cán bộ --</option></select>
      <button class="btn" onclick="addCanBo()">Thêm</button>
    </div>
    <div id="dsCanBo"></div>

    <div class="row">
  <label>Địa điểm công tác:</label>
  <input id="diadiem" placeholder="Nhập địa điểm..." style="flex:1; min-width:350px;">
</div>

    <div class="row">
      <label>Công tác từ ngày:</label>
      <input type="date" id="startDate">
      <span style="margin:0 5px;">đến</span>
      <input type="date" id="endDate">
      <button class="btn" onclick="tinhNgay()">Tính số ngày</button>
    </div>
    <p><b>Số ngày công tác: <span id="soNgay">0</span> ngày</b></p>

    <table id="chiTable">
      <thead>
        <tr>
          <th>STT</th>
          <th>Nội dung</th>
          <th>Số tiền (VNĐ)</th>
          <th>Xóa</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>

    <div class="row">
      <input id="noidung" placeholder="Nội dung chi" style="flex:1; min-width:300px;">
      <input id="sotien" type="number" placeholder="Số tiền">
      <button class="btn" onclick="addChi()">Thêm</button>
    </div>

    <div style="display:flex; justify-content:space-between; margin-top:10px;">
  <button class="btn" onclick="showPhieu()">Xem giấy đề nghị</button>
  <button class="btn gray" onclick="logout()">Đăng xuất</button>
</div>
  </div>

  <!-- ===== Phiếu ===== -->
  <div id="phieuArea" class="container hidden">
    <div class="header">
    <div class="logo">
      <img src="logo-h2soft.jpg" alt="Logo H2SOFT">
    </div>
    <div style="text-align:center">
      <p><b>CỘNG HOÀ XÃ HỘI CHỦ NGHĨA VIỆT NAM</b></p>
      <p>Độc lập - Tự do - Hạnh phúc</p>
      <p>-----o0o-----</p>
    </div>
    </div>

    

    <h2>GIẤY ĐỀ NGHỊ THANH TOÁN</h2>
    <p>Kính gửi: Ban lãnh đạo công ty Cổ Phần Công Nghệ H2soft</p>
    <p>Tôi là: <span id="ten"></span></p>
    <p>Địa điểm công tác: <span id="ddCongTac"></span></p>
    <p>Đề nghị công ty thanh toán cho tôi các khoản sau:</p>

    <table id="dsChi">
      <thead>
        <tr><th>STT</th><th>Nội dung</th><th>Số tiền (VNĐ)</th></tr>
      </thead>
      <tbody></tbody>
      <tfoot>
        <tr><td colspan="2"><b>Tổng cộng</b></td><td class="right" id="tong"></td></tr>
      </tfoot>
    </table>

    <p>Viết bằng chữ: <i id="bangChu"></i></p>

    <!-- Ngày tháng năm: đặt lên phía trên ô "Thủ trưởng đơn vị" (ở cột phải) -->
    <div class="dateRowSingle">
  <div id="ngayThangNam"></div>
</div>
        <br>
        

    <div class="sign">
      <div>Người đề nghị<br><br><br><br><br><span id="signName"></span></div>
      <div>Kế toán<br><br><br><br><br>Nguyễn Thị Kim Thoa</div>
      <div>Thủ trưởng đơn vị<br><br><br><br><br>..................</div>
    </div>
          <br>
          <br>
          <br>
          <br>
   <div style="display:flex; justify-content:space-between; margin-top:10px;">
  <button class="btn" onclick="back()">Quay lại</button>
  <button class="btn gray" onclick="logout()">Đăng xuất</button>
</div>
  </div>

  

  <script>
    // ====== Quản lý user ======
    let users = [];
    let currentUser = null;
    let soNgayCongTac = 0;
    let selectedCanBo = [];
    let diaDiem = "";
    let currentEditingUsername = null;

    function getCongTacPhi(user, diaDiem) {
  if (!diaDiem) return user.tieuvat; // fallback về mức mặc định

  let diaDiemLower = diaDiem.toLowerCase();
  let diaDiemDacBiet = ["ninh hòa", "vạn ninh", "cam lâm", "cam ranh", "yang bay"];

  // kiểm tra nếu diaDiem nhập thuộc danh sách đặc biệt
  let isSpecial = diaDiemDacBiet.some(d => diaDiemLower.includes(d));

  if (isSpecial) {
    if (user.username === "hai" || user.username === "hoan") {
      return 80000;
    } else {
      return 40000;
    }
  }

  // nếu không thuộc địa điểm đặc biệt thì lấy mức tiêu vặt mặc định
  return user.tieuvat;
}

    function loadUsers() {
      let data = localStorage.getItem("users");
      if(data) {
        users = JSON.parse(data);
      } else {
        users = [
          { username: "tung", password: "123", hoten: "Phạm Tiến Tùng", tieuvat: 100000, chitieu: [] },
          { username: "hai", password: "123", hoten: "Đinh Tuấn Hải", tieuvat: 200000, chitieu: [] }
        ];
        saveUsers();
      }
    }
    function saveUsers() { localStorage.setItem("users", JSON.stringify(users)); }
    function reloadCanBoList() {
      let sel = document.getElementById("cbSelect");
      sel.innerHTML = '<option value="">-- Chọn cán bộ --</option>';
      users.forEach(u => {
        let opt = document.createElement("option");
        opt.value = u.username;
        opt.textContent = u.hoten;
        sel.appendChild(opt);
      });
    }

    window.onload = () => { loadUsers(); reloadCanBoList(); };

    // ====== Login/Register ======
    function login() {
      let u = document.getElementById("username").value;
      let p = document.getElementById("password").value;
      let user = users.find(x => x.username === u && x.password === p);
      if(user) {
        currentUser = user;
        document.getElementById("loginArea").classList.add("hidden");
        document.getElementById("inputArea").classList.remove("hidden");
        document.getElementById("userName").innerText = currentUser.hoten;
        renderChiTable();
      } else {
        document.getElementById("loginMsg").innerText = "Sai tài khoản hoặc mật khẩu!";
      }
    }
    function showRegister() {
      document.getElementById("loginArea").classList.add("hidden");
      document.getElementById("registerArea").classList.remove("hidden");
    }
    function backLogin() {
      hideEditPanel();
      document.getElementById("registerArea").classList.add("hidden");
      document.getElementById("manageArea").classList.add("hidden");
      document.getElementById("loginArea").classList.remove("hidden");
    }
    function logout() {
  currentUser = null;
  selectedCanBo = [];
  soNgayCongTac = 0;
  diaDiem = "";

  // Ẩn tất cả các khu vực
  document.getElementById("inputArea").classList.add("hidden");
  document.getElementById("phieuArea").classList.add("hidden");
  document.getElementById("manageArea").classList.add("hidden");
  document.getElementById("registerArea").classList.add("hidden");

  // Hiện lại màn hình đăng nhập
  document.getElementById("loginArea").classList.remove("hidden");

  // Reset form đăng nhập
  document.getElementById("username").value = "";
  document.getElementById("password").value = "";
  document.getElementById("loginMsg").innerText = "";
}


    function register() {
      let hoten = document.getElementById("regHoten").value.trim();
      let user = document.getElementById("regUser").value.trim();
      let pass = document.getElementById("regPass").value.trim();
      let tieuvat = parseInt(document.getElementById("regTieuvat").value);
      if(!hoten || !user || !pass || !tieuvat) {
        document.getElementById("regMsg").innerText = "Vui lòng nhập đầy đủ thông tin!";
        return;
      }
      if(users.find(x => x.username === user)) {
        document.getElementById("regMsg").innerText = "Tên đăng nhập đã tồn tại!";
        return;
      }
      users.push({ username: user, password: pass, hoten: hoten, tieuvat: tieuvat, chitieu: [] });
      saveUsers(); reloadCanBoList();
      document.getElementById("regMsg").style.color = "green";
      document.getElementById("regMsg").innerText = "Đăng ký thành công!";
      setTimeout(backLogin, 1500);
    }

    // ====== Quản lý TK ======
    function showManage() {
      document.getElementById("loginArea").classList.add("hidden");
      document.getElementById("manageArea").classList.remove("hidden");
      renderUserTable();
    }
    function renderUserTable() {
      let tbody = document.querySelector("#userTable tbody");
      tbody.innerHTML = "";
      users.forEach((u,i) => {
        let tr = document.createElement("tr");
        tr.innerHTML = `
          <td>${i+1}</td>
          <td>${u.hoten}</td>
          <td>${u.username}</td>
          <td>${u.tieuvat.toLocaleString()} đ</td>
          <td>
            <button class="delBtn" onclick="deleteUser('${u.username}')">Xóa</button>
            <button class="btn green" onclick="openEditPanel('${u.username}')">Sửa</button>
          </td>
        `;
        tbody.appendChild(tr);
      });
    }
    function deleteUser(username) {
      if(confirm("Xóa tài khoản " + username + "?")) {
        users = users.filter(u => u.username !== username);
        saveUsers(); reloadCanBoList(); renderUserTable();
      }
    }

    // Edit panel
    function openEditPanel(username) {
      let user = users.find(u => u.username === username);
      if(!user) return;
      currentEditingUsername = username;
      document.getElementById("editArea").classList.remove("hidden");
      const tbody = document.querySelector("#editTable");
      tbody.innerHTML = `
        <thead><tr><th>Trường</th><th>Giá trị (click để sửa)</th></tr></thead>
        <tbody>
        <tr>
          <td style="width:35%"><b>Họ tên</b></td>
          <td class="editable" data-field="hoten">${user.hoten}</td>
        </tr>
        <tr>
          <td><b>Tài khoản</b></td>
          <td>${user.username}</td>
        </tr>
        <tr>
          <td><b>Mức tiêu vặt/ngày (VNĐ)</b></td>
          <td class="editable" data-field="tieuvat">${user.tieuvat}</td>
        </tr>
        </tbody>
      `;
      document.querySelectorAll("#editTable td.editable").forEach(td=>{
        td.addEventListener("click", function handler(){
          if(td.querySelector("input")) return;
          let field = td.dataset.field;
          let old = td.innerText.replace(/,/g,"").trim();
          let input = document.createElement("input");
          input.type = (field === "tieuvat") ? "number" : "text";
          input.value = old;
          td.innerHTML = "";
          td.appendChild(input);
          input.focus();
          input.addEventListener("blur", ()=>{
            let v = input.value.trim();
            td.innerText = (v===""? "" : v);
          });
          input.addEventListener("keydown", e=>{
            if(e.key==="Enter") input.blur();
            if(e.key==="Escape") td.innerText = old;
          });
        });
      });
    }
    function cancelEdit(){ currentEditingUsername=null; hideEditPanel(); }
    function hideEditPanel(){ document.getElementById("editArea").classList.add("hidden"); }
    function saveEditPanel(){
      if(!currentEditingUsername) return;
      let user = users.find(u=>u.username===currentEditingUsername);
      if(!user) return;
      let newHoten = document.querySelector("#editTable td[data-field='hoten']").innerText.trim();
      let newTieuvat = parseInt(document.querySelector("#editTable td[data-field='tieuvat']").innerText.trim());
      if(newHoten===""){ alert("Họ tên không được để trống!"); return; }
      if(isNaN(newTieuvat)||newTieuvat<=0){ alert("Mức tiêu vặt phải là số > 0!"); return; }
      user.hoten=newHoten; user.tieuvat=newTieuvat;
      saveUsers(); reloadCanBoList(); renderUserTable();
      currentEditingUsername=null; hideEditPanel(); alert("Cập nhật thành công!");
    }

    // ====== Cán bộ + chi tiêu ======
    function addCanBo() {
      let sel=document.getElementById("cbSelect");
      let val=sel.value;
      if(val && !selectedCanBo.includes(val)){ selectedCanBo.push(val); renderCanBo(); }
    }
    function removeCanBo(username){ selectedCanBo=selectedCanBo.filter(c=>c!==username); renderCanBo(); }
    function renderCanBo(){
      let div=document.getElementById("dsCanBo"); div.innerHTML="";
      selectedCanBo.forEach(c=>{
        let u=users.find(x=>x.username===c);
        if(u){
          let span=document.createElement("div");
          span.className="selected-cb";
          span.innerHTML=`${u.hoten} <span onclick="removeCanBo('${c}')">x</span>`;
          div.appendChild(span);
        }
      });
    }
    function tinhNgay(){
      let sVal = document.getElementById("startDate").value;
      let eVal = document.getElementById("endDate").value;
      let s = new Date(sVal);
      let e = new Date(eVal);
      if(!isNaN(s) && !isNaN(e) && e >= s){
        soNgayCongTac = Math.floor((e - s)/(1000*60*60*24)) + 1;
        document.getElementById("soNgay").innerText = soNgayCongTac;
      } else {
        alert("Vui lòng chọn ngày hợp lệ (end >= start).");
      }
    }

    function addChi() {
      if(!currentUser){ alert("Vui lòng đăng nhập trước."); return; }
      let nd=document.getElementById("noidung").value.trim();
      let st=parseInt(document.getElementById("sotien").value);
      if(nd && st>0) {
        currentUser.chitieu.push({noidung:nd,sotien:st}); saveUsers(); renderChiTable();
        document.getElementById("noidung").value=""; document.getElementById("sotien").value="";
      } else {
        alert("Vui lòng điền nội dung và số tiền (>0).");
      }
    }
    function delChi(i) { currentUser.chitieu.splice(i,1); saveUsers(); renderChiTable(); }

   function renderChiTable() {
  if(!currentUser) return;
  let tbody = document.querySelector("#chiTable tbody");
  tbody.innerHTML = "";
  currentUser.chitieu.forEach((c, i) => {
    let tr = document.createElement("tr");
    tr.innerHTML = `
      <td>${i+1}</td>
      <td class="editable" data-field="noidung" data-index="${i}" style="text-align:center">${c.noidung}</td>
      <td class="editable" data-field="sotien" data-index="${i}" style="text-align:center">${c.sotien.toLocaleString()}</td>
      <td><button class="delBtn" onclick="delChi(${i})">Xoá</button></td>
    `;
    tbody.appendChild(tr);
  });

  // Gắn sự kiện click cho tất cả ô editable
  tbody.querySelectorAll("td.editable").forEach(td => {
    td.addEventListener("click", function handler() {
      // nếu đang có input thì không tạo thêm
      if (td.querySelector("input")) return;

      let oldValue = td.innerText.replace(/,/g, "").trim();
      let field = td.dataset.field;
      let index = parseInt(td.dataset.index, 10);

      let input = document.createElement("input");
      input.type = (field === "sotien") ? "number" : "text";
      input.value = oldValue;
      input.style.width = "100%";
      input.style.boxSizing = "border-box";

      td.innerHTML = "";
      td.appendChild(input);
      input.focus();

      // Khi mất focus -> lưu thay đổi (nếu hợp lệ)
      input.addEventListener("blur", () => {
        let newVal = input.value.trim();

        if (field === "sotien") {
          let num = parseInt(newVal, 10);
          if (!isNaN(num) && num >= 0) {
            currentUser.chitieu[index].sotien = num;
            td.innerText = num.toLocaleString();
          } else {
            // giữ lại giá trị cũ nếu nhập không hợp lệ
            td.innerText = parseInt(oldValue, 10).toLocaleString();
          }
        } else { // noidung
          if (newVal !== "") {
            currentUser.chitieu[index].noidung = newVal;
            td.innerText = newVal;
          } else {
            td.innerText = oldValue;
          }
        }
        saveUsers(); // lưu vào localStorage
      });

      // Phím tắt: Enter = lưu (blur), Escape = hủy
      input.addEventListener("keydown", (e) => {
        if (e.key === "Enter") input.blur();
        if (e.key === "Escape") {
          td.innerText = oldValue;
        }
      });
    });
  });
}

    // ====== In phiếu ======
    function showPhieu() {
      if(!currentUser){ alert("Vui lòng đăng nhập để tạo phiếu."); return; }
      diaDiem = document.getElementById("diadiem").value.trim();
      document.getElementById("inputArea").classList.add("hidden");
      document.getElementById("phieuArea").classList.remove("hidden");
      renderPhieu();
    }
    function back() {
      document.getElementById("phieuArea").classList.add("hidden");
      document.getElementById("inputArea").classList.remove("hidden");
    }

    function renderPhieu() {
      // Tên cán bộ (danh sách)
      let dsTen = selectedCanBo.map(c=>{let u=users.find(x=>x.username===c);return u?u.hoten:"";}).filter(x=>x);
      document.getElementById("ten").innerText = dsTen.join(", ") || currentUser.hoten;
      document.getElementById("ddCongTac").innerText = diaDiem||"..................";
      document.getElementById("signName").innerText = currentUser.hoten;

      // Danh sách chi tiêu (của currentUser) + tiêu vặt cho CB được chọn
      let tbody=document.querySelector("#dsChi tbody"); tbody.innerHTML="";
      let tong=0, stt=0;
      currentUser.chitieu.forEach((c)=>{ tong+=c.sotien; stt++;
        let row = document.createElement("tr");
        row.innerHTML = `<td>${stt}</td><td style="text-align:center">${c.noidung}</td><td class="right">${c.sotien.toLocaleString()}</td>`;
        tbody.appendChild(row);
      });

    selectedCanBo.forEach(name => {
  let u = users.find(x => x.username === name);
  if (u && soNgayCongTac > 0) {
    let congTacPhi = getCongTacPhi(u, diaDiem);  // lấy mức công tác phí đúng
    let tienTV = congTacPhi * soNgayCongTac;
    tong += tienTV;

    stt++; // tăng số thứ tự
    let row = document.createElement("tr");
    row.innerHTML = `
      <td>${stt}</td>
      <td style="text-align:left">
        Tiêu vặt cho ${u.hoten} (${soNgayCongTac} ngày × ${congTacPhi.toLocaleString()} đ)
      </td>
      <td class="right">${tienTV.toLocaleString()}</td>
    `;
    tbody.appendChild(row);
  }
});

      document.getElementById("tong").innerText = tong.toLocaleString();
      document.getElementById("bangChu").innerText = convertNumberToVietnameseWords(tong);
      // Ngày tháng năm: hiện ngày hiện tại (có thể chỉnh nếu muốn)
      document.getElementById("ngayThangNam").innerText = formatTodayForPrint();
    }

    // ====== Hàm chuyển số sang chữ (Tiếng Việt) ======
    // Trả về chuỗi chỉ bằng chữ (ví dụ: "Một triệu hai trăm ba mươi bốn nghìn đồng")
    function convertNumberToVietnameseWords(amount) {
      if (isNaN(amount) || amount === 0) return "Không đồng";
      const units = ["","một","hai","ba","bốn","năm","sáu","bảy","tám","chín"];
      const scales = ["","nghìn","triệu","tỷ","nghìn tỷ","triệu tỷ","tỷ tỷ"];
      function readThreeDigits(num) {
        let hundred = Math.floor(num/100);
        let tenUnit = num % 100;
        let ten = Math.floor(tenUnit/10);
        let unit = tenUnit % 10;
        let parts = [];
        if (hundred>0) parts.push(units[hundred] + " trăm");
        if (ten>1) {
          parts.push(units[ten] + " mươi" + (unit===1 ? " mốt" : (unit===4 ? " tư" : (unit===5 ? " lăm" : (unit>0 ? " " + units[unit] : "")))));
        } else if (ten===1) {
          parts.push("mười" + (unit===0 ? "" : (unit===5 ? " lăm" : " " + units[unit])));
        } else if (ten===0 && unit>0) {
          if (hundred>0) parts.push("lẻ " + (unit===5 ? "năm" : units[unit]));
          else parts.push(units[unit]);
        } else if (ten>1 && unit===0) {
          // nothing extra
        }
        return parts.join(" ");
      }
      // Split into groups of 3 digits from right
      let groups = [];
      let n = Math.abs(Math.floor(amount));
      while (n > 0) { groups.push(n % 1000); n = Math.floor(n / 1000); }
      let words = [];
      for (let i = groups.length - 1; i >= 0; i--) {
        let g = groups[i];
        if (g === 0) {
          // nếu nằm giữa các group khác không thì vẫn cần đánh dấu "không" khi các sau group có giá trị
          // nhưng để đơn giản, ta bỏ qua group 0 trừ khi tất cả đều 0
        } else {
          let part = readThreeDigits(g);
          if (part) {
            part = part.trim();
            if (scales[i]) part += " " + scales[i];
            words.push(part);
          }
        }
      }
      // Xử lý một vài quy tắc tiếng Việt đặc biệt (mốt/tư/lăm)
      let result = words.join(" ").replace(/\s+/g," ").trim();
      // normalize: một -> Một đầu câu? keep lowercase as user wanted
      // thêm "đồng" ở cuối
      return capitalizeFirstLetter(result) + " đồng";
    }

    function capitalizeFirstLetter(s){
      if(!s) return s;
      return s.charAt(0).toUpperCase() + s.slice(1);
    }

    // Hàm lấy ngày hiện tại dạng "Ngày dd tháng mm năm yyyy"
    function formatTodayForPrint(){
      let d = new Date();
      let dd = d.getDate(); let mm = d.getMonth()+1; let yy = d.getFullYear();
      return `Ngày ${dd} tháng ${mm} năm ${yy}`;
    }
   /* function formatDateFromEndPlus2(){
  let eVal = document.getElementById("endDate").value;
  let d = eVal ? new Date(eVal) : new Date();
  d.setDate(d.getDate() + 2);
  let dd = d.getDate();
  let mm = d.getMonth() + 1;
  let yy = d.getFullYear();
  return `Ngày ${dd} tháng ${mm} năm ${yy}`;
}*/
    // OPTIONAL: nếu muốn hiển thị ngày dựa trên endDate (nếu đã chọn), có thể dùng:
    // function formatDateForPrintFromInput() { ... }

    // Helper: khi load demo, nếu chưa đăng nhập thì chỉ render danh sách rỗng
    // Bạn có thể thêm tính năng in ấn (window.print()) nếu cần.
  </script>
</body>
</html>
