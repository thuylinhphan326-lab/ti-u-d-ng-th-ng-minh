function formatMoney(num) {
    return num.toLocaleString("vi-VN") + " VNĐ";
}

function round1000(n) {
    return Math.floor(n / 1000) * 1000;
}

function generatePlan() {
    let income = Number(document.getElementById("income").value);
    let age = Number(document.getElementById("age").value);
    let group = document.getElementById("group").value;

    if (!income || !age) {
        alert("Vui lòng nhập đầy đủ thông tin!");
        return;
    }

    let data = getCategories(group);
    let html = "";

    html += `<h2 class='section-title'>📌 Tổng thu nhập: ${formatMoney(income)}</h2>`;

    for (let [name, percent] of Object.entries(data)) {
        let total = round1000(income * percent);

        html += `
            <div class="planner-box">
                <h3>${name}: <span style="color:#8b4a22">${formatMoney(total)}</span></h3>
                <ul>
                    ${genSub(name, total).join("")}
                </ul>
            </div>
        `;
    }

    document.getElementById("result").innerHTML = html;
}

function getCategories(group) {
    if (group === "student") {
        return {
            "Ăn uống & đi lại": 0.35,
            "Học tập – dụng cụ": 0.25,
            "Giải trí – bạn bè": 0.20,
            "Tiết kiệm": 0.20
        };
    }

    if (group === "college") {
        return {
            "Thuê trọ": 0.32,
            "Ăn uống": 0.28,
            "Học tập – tài liệu": 0.18,
            "Giao lưu – giải trí": 0.12,
            "Tiết kiệm": 0.10
        };
    }

    if (group === "worker") {
        return {
            "Nhà ở / Hỗ trợ gia đình": 0.30,
            "Ăn uống – đi lại": 0.25,
            "Công việc – giao lưu": 0.15,
            "Mua sắm cá nhân": 0.15,
            "Tiết kiệm – đầu tư": 0.15
        };
    }

    if (group === "housewife") {
        return {
            "Chợ – ăn uống": 0.40,
            "Tiền nhà – điện nước": 0.25,
            "Con cái – học hành": 0.20,
            "Đồ gia dụng": 0.10,
            "Tiết kiệm": 0.05
        };
    }

    return {
        "Sức khỏe – thuốc men": 0.40,
        "Ăn uống": 0.25,
        "Giao lưu": 0.15,
        "Tiết kiệm – dự phòng": 0.10,
        "Quà cháu chắt": 0.10
    };
}

function genSub(category, total) {
    let ratio = {};

    if (category.includes("Ăn uống")) {
        ratio = {
            "Bữa chính": 0.55,
            "Ăn vặt – trà sữa": 0.25,
            "Dự phòng": 0.20
        };
    } else if (category.includes("Học")) {
        ratio = {
            "Sách vở": 0.40,
            "Dụng cụ học tập": 0.35,
            "Tài liệu thêm": 0.25
        };
    } else if (category.includes("Sức khỏe")) {
        ratio = {
            "Thuốc men": 0.50,
            "Khám định kỳ": 0.30,
            "Dự phòng": 0.20
        };
    } else {
        ratio = {
            "Khoản chính": 0.45,
            "Khoản phụ": 0.30,
            "Dự phòng": 0.25
        };
    }

    let list = [];
    for (let [name, r] of Object.entries(ratio)) {
        let money = round1000(total * r);
        list.push(`<li>${name}: <b>${formatMoney(money)}</b></li>`);
    }
    return list;
}
