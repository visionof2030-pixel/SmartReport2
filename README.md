<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=5.0, user-scalable=yes">
<title>أداة إصدار التقارير والشواهد</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
@import url('https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;500;600;700;800;900&display=swap');
*{margin:0;padding:0;box-sizing:border-box; -webkit-tap-highlight-color: transparent;}
html,body{font-family:'Cairo',sans-serif;background: linear-gradient(135deg, #f0f9f6 0%, #e8f4f0 50%, #d4ebe2 100%);direction:rtl;overflow-x:hidden;min-height:100vh;-webkit-text-size-adjust:100%; -moz-text-size-adjust:100%; -ms-text-size-adjust:100%; text-size-adjust:100%; touch-action: manipulation;}
.wrapper{max-width:900px;margin:auto;padding:20px;width:100%;}

/* شريط الأخبار العلوي - محسن للأجهزة المحمولة */
.top-marquee{
position:fixed;top:0;left:0;right:0;width:100%;background:linear-gradient(135deg, #022e22 0%, #044a35 100%);color:#fff;
padding:10px 5px;font-size:12px;z-index:300;overflow:hidden;height:45px;
white-space:nowrap;border-bottom:3px solid #ffd166;box-shadow:0 4px 12px rgba(2, 46, 34, 0.25);
display:flex;align-items:center;
}
.marquee-inner{
display:inline-block;
padding-left:2%;
animation:newsScroll 30s linear infinite;
color:#e8f4f0;font-weight:500;
}
@keyframes newsScroll{
0%{transform:translateX(-100%);}
100%{transform:translateX(100%);}
}
.top-marquee:hover .marquee-inner{animation-play-state:paused;}

/* شريط التحكم العلوي - متجاوب تماماً */
.control-bar{
position:fixed;top:45px;left:0;right:0;width:100%;z-index:250;
background:linear-gradient(135deg, #ffffff 0%, #f5fcf9 100%);
padding:12px 15px;display:flex;justify-content:space-between;align-items:center;
box-shadow:0 4px 15px rgba(4, 74, 53, 0.12);border-bottom:2px solid #d0e6de;
backdrop-filter:blur(5px);
}

/* تحسين تصميم الهيدر مع إضافة التاريخ */
.header-controls {
display: flex;
align-items: center;
gap: 15px;
flex: 1;
flex-wrap: wrap;
}

/* تاريخ التحويل - جديد */
.date-toggle-container {
display: flex;
flex-direction: column;
gap: 4px;
background: linear-gradient(135deg, #e8f4f0 0%, #d4ebe2 100%);
padding: 6px 10px;
border-radius: 10px;
font-size: 12px;
border-right: 4px solid #ffd166;
box-shadow: 0 3px 8px rgba(6, 109, 77, 0.15);
min-width: 220px;
}

.date-toggle-btn {
background: linear-gradient(135deg, #066d4d 0%, #05553d 100%);
color: white;
border: none;
padding: 4px 8px;
border-radius: 6px;
font-size: 11px;
cursor: pointer;
display: flex;
align-items: center;
gap: 5px;
transition: all 0.3s;
}

.date-toggle-btn:hover {
background: linear-gradient(135deg, #05553d 0%, #044a35 100%);
transform: translateY(-2px);
}

.date-display {
font-weight: 700;
color: #044a35;
text-align: center;
font-size: 12px;
margin-bottom: 5px;
}

/* تصميم عنوان التطبيق المعدل */
.app-title {
background: linear-gradient(135deg, #e8f4f0 0%, #d4ebe2 100%);
color: #044a35;
padding: 6px 10px;
border-radius: 10px;
font-size: 12px;
font-weight: 800;
border-right: 4px solid #ffd166;
display: flex;
align-items: center;
gap: 8px;
box-shadow: 0 3px 8px rgba(6, 109, 77, 0.15);
flex: 1;
text-align: center;
justify-content: center;
cursor: pointer;
transition: all 0.3s;
position: relative;
}

.app-title:hover {
transform: translateY(-2px);
box-shadow: 0 5px 12px rgba(6, 109, 77, 0.25);
}

/* زر تنفيذ المعلم - قابل للنقر */
.app-title {
    cursor: pointer;
    transition: all 0.3s;
    position: relative;
}

.app-title:hover {
    transform: translateY(-2px);
    box-shadow: 0 5px 12px rgba(6, 109, 77, 0.25);
}

/* نافذة الإدارة المخفية */
.admin-panel {
    display: none;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: white;
    padding: 25px;
    border-radius: 15px;
    box-shadow: 0 15px 40px rgba(0, 0, 0, 0.3);
    z-index: 2000;
    width: 90%;
    max-width: 500px;
    max-height: 85vh;
    overflow-y: auto;
    border: 3px solid #ffd166;
}

.admin-panel.show {
    display: block;
    animation: fadeIn 0.4s cubic-bezier(0.68, -0.55, 0.27, 1.55);
}

@keyframes fadeIn {
    from { opacity: 0; transform: translate(-50%, -60%); }
    to { opacity: 1; transform: translate(-50%, -50%); }
}

.overlay {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.7);
    z-index: 1999;
}

.overlay.show {
    display: block;
}

.admin-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
    padding-bottom: 15px;
    border-bottom: 2px solid #e0f0ea;
}

.admin-header h3 {
    color: #044a35;
    font-size: 20px;
    font-weight: 800;
    display: flex;
    align-items: center;
    gap: 10px;
}

.close-admin {
    background: none;
    border: none;
    font-size: 24px;
    color: #066d4d;
    cursor: pointer;
    width: 30px;
    height: 30px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    transition: all 0.3s;
}

.close-admin:hover {
    background-color: #e8f4f0;
}

/* تصميم حقول المفاتيح */
.key-field {
    margin-bottom: 20px;
    background: #f8fdfa;
    padding: 15px;
    border-radius: 10px;
    border: 2px solid #d4ebe2;
    transition: all 0.3s;
}

.key-field:hover {
    border-color: #066d4d;
    transform: translateY(-3px);
    box-shadow: 0 4px 12px rgba(6, 109, 77, 0.1);
}

.key-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
}

.key-header label {
    font-weight: 800;
    color: #044a35;
    display: flex;
    align-items: center;
    gap: 8px;
}

.key-actions {
    display: flex;
    gap: 8px;
}

.check-key-btn, .toggle-key-btn {
    padding: 6px 12px;
    border: none;
    border-radius: 6px;
    font-size: 12px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.3s;
    display: flex;
    align-items: center;
    gap: 5px;
}

.check-key-btn {
    background: linear-gradient(135deg, #5a67d8 0%, #4c51bf 100%);
    color: white;
}

.check-key-btn:hover {
    background: linear-gradient(135deg, #4c51bf 0%, #434190 100%);
}

.toggle-key-btn {
    background: linear-gradient(135deg, #ffd166 0%, #f0ad4e 100%);
    color: #333;
}

.toggle-key-btn:hover {
    background: linear-gradient(135deg, #f0ad4e 0%, #ec971f 100%);
}

.toggle-key-btn.active {
    background: linear-gradient(135deg, #25D366 0%, #128C7E 100%);
    color: white;
}

.key-status {
    font-size: 11px;
    padding: 4px 8px;
    border-radius: 4px;
    margin-top: 8px;
    display: none;
    align-items: center;
    gap: 5px;
}

.key-status.active {
    display: flex;
    background: #e8f4f0;
    color: #044a35;
    border-right: 3px solid #25D366;
}

.key-status.inactive {
    display: flex;
    background: #fee;
    color: #d9534f;
    border-right: 3px solid #d9534f;
}

.key-input {
    width: 100%;
    padding: 12px;
    border: 2px solid #d4ebe2;
    border-radius: 8px;
    font-size: 14px;
    font-family: monospace;
    background: white;
    transition: all 0.3s;
}

.key-input:focus {
    outline: none;
    border-color: #066d4d;
    box-shadow: 0 0 0 3px rgba(6, 109, 77, 0.15);
}

/* إجراءات المفاتيح */
.key-actions-global {
    display: flex;
    gap: 10px;
    margin: 25px 0;
    flex-wrap: wrap;
}

.global-action-btn {
    flex: 1;
    min-width: 120px;
    padding: 12px;
    border: none;
    border-radius: 10px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.3s;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

.activate-all-btn {
    background: linear-gradient(135deg, #066d4d 0%, #05553d 100%);
    color: white;
}

.activate-all-btn:hover {
    background: linear-gradient(135deg, #05553d 0%, #044a35 100%);
    transform: translateY(-3px);
}

.deactivate-all-btn {
    background: linear-gradient(135deg, #f0ad4e 0%, #ec971f 100%);
    color: #333;
}

.deactivate-all-btn:hover {
    background: linear-gradient(135deg, #ec971f 0%, #d58512 100%);
    transform: translateY(-3px);
}

.save-keys-btn {
    background: linear-gradient(135deg, #5a67d8 0%, #4c51bf 100%);
    color: white;
}

.save-keys-btn:hover {
    background: linear-gradient(135deg, #4c51bf 0%, #434190 100%);
    transform: translateY(-3px);
}

/* زر التعبئة الذكية ٢ */
#aiFillBtn2 {
    background: linear-gradient(135deg, #9b59b6 0%, #8e44ad 100%);
    position: relative;
    display: none; /* مخفي بشكل افتراضي */
}

#aiFillBtn2:hover {
    background: linear-gradient(135deg, #8e44ad 0%, #7d3c98 100%);
}

#aiFillBtn2.show {
    display: flex;
}

/* زر خاص يظهر بعد 3 نقرات على عنوان المعلم */
#specialBtn {
    display: none;
}

#specialBtn.show {
    display: flex;
}

/* مجموعة الأزرار المعدلة - مرتبة في صفين */
.btn-group {
    display: flex;
    flex-direction: column;
    gap: 8px;
    width: 100%;
    max-width: 600px;
    margin: 0 auto;
}

.btn-row {
    display: flex;
    justify-content: space-between;
    gap: 8px;
    width: 100%;
}

button.main-btn{
background:linear-gradient(135deg, #066d4d 0%, #05553d 100%);color:#fff;border:none;
padding:12px 10px;font-size:13px;border-radius:12px;cursor:pointer;
transition:all 0.3s ease;font-weight:700;position:relative;overflow:hidden;
box-shadow:0 4px 10px rgba(6, 109, 77, 0.25);display:flex;flex-direction:column;align-items:center;justify-content:center;
border:1px solid rgba(255,255,255,0.1);flex:1;
min-height: 65px;
}
button.main-btn::after{
content:'';position:absolute;top:0;left:0;width:100%;height:100%;
background:linear-gradient(90deg, transparent, rgba(255,255,255,0.1), transparent);
transform:translateX(-100%);
}
button.main-btn:hover::after{animation:buttonShine 0.6s;}
@keyframes buttonShine{100%{transform:translateX(100%);}}
button.main-btn:hover{
background:linear-gradient(135deg, #05553d 0%, #044a35 100%);transform:translateY(-3px);
box-shadow:0 6px 15px rgba(6, 109, 77, 0.35);
}
button.main-btn:active{transform:translateY(-1px);}

.btn-icon{font-size:18px; margin-bottom:5px;}
.btn-text{font-size:12px;font-weight:800; text-align:center; line-height:1.2;}

/* زر حفظ بيانات المعلم خاص */
#saveTeacherBtn{background:linear-gradient(135deg, #2a7b5e 0%, #1e6b4f 100%);}
#saveTeacherBtn:hover{background:linear-gradient(135deg, #1e6b4f 0%, #15563f 100%);}

/* زر الدعم الفني */
#supportBtn{background:linear-gradient(135deg, #5a67d8 0%, #4c51bf 100%);}
#supportBtn:hover{background:linear-gradient(135deg, #4c51bf 0%, #434190 100%);}

/* زر المسح */
#clearBtn{background:linear-gradient(135deg, #f0ad4e 0%, #ec971f 100%);}
#clearBtn:hover{background:linear-gradient(135deg, #ec971f 0%, #d58512 100%);}

/* زر الذكاء الاصطناعي المعدل - الأصفر والأزرق */
#aiFillBtn{background:linear-gradient(135deg, #ffd166 0%, #4d96ff 100%); position: relative;}
#aiFillBtn:hover{background:linear-gradient(135deg, #ffc145 0%, #2d7dfd 100%);}

/* تصميم خاص لأزرار PDF وواتساب */
#pdfBtn{background:linear-gradient(135deg, #d9534f 0%, #c9302c 100%);}
#pdfBtn:hover{background:linear-gradient(135deg, #c9302c 0%, #ac2925 100%);}

#whatsappBtn{background:linear-gradient(135deg, #25D366 0%, #128C7E 100%);}
#whatsappBtn:hover{background:linear-gradient(135deg, #128C7E 0%, #075E54 100%);}

/* ========== تحسين واجهة الإدخال - تصميم عصري 2026 ========== */
.input-section{
background:#ffffff;padding:25px;border-radius:20px;margin-top:120px;
border:2px solid #e0f0ea;box-shadow:0 10px 30px rgba(4, 74, 53, 0.12);
position:relative;overflow:hidden;
}
.input-section::before{
content:'';position:absolute;top:0;right:0;width:100%;height:5px;
background:linear-gradient(to left, #066d4d, #ffd166, #25D366);
}

.input-section h2{
color:#044a35;font-size:24px;margin-bottom:30px;padding-bottom:15px;
border-bottom:3px solid #e0f0ea;text-align:center;font-weight:900;
position:relative;
}
.input-section h2::after{
content:'';position:absolute;bottom:-3px;right:50%;transform:translateX(50%);
width:120px;height:3px;background:linear-gradient(to left, #066d4d, #ffd166);
border-radius:2px;
}

/* تصميم عصري للحقول */
.form-group{margin-bottom:25px;position:relative;}
.form-group label{
font-size:16px;font-weight:800;margin-bottom:10px;display:block;color:#083024;
display:flex;align-items:center;gap:12px;padding-right:8px;
position:relative;
}
.form-group label i{
color:#066d4d;font-size:16px;background:#f0f9f6;padding:7px;border-radius:10px;
border:1px solid #d4ebe2;box-shadow:0 2px 5px rgba(0,0,0,0.05);
}

.form-group label::before{
content:'';width:8px;height:8px;background:#ffd166;border-radius:50%;
display:inline-block;margin-left:6px;box-shadow:0 0 6px #ffd166;
}

input,select,textarea{
width:100%;padding:16px;margin-top:8px;border:2px solid #d4ebe2;border-radius:12px;
font-size:18px;background:#f9fcfb;transition:all 0.3s;font-family:'Cairo', sans-serif;
color:#083024;box-shadow:inset 0 2px 8px rgba(0,0,0,0.05);-webkit-appearance:none;
}
input:focus,select:focus,textarea:focus{
outline:none;border-color:#066d4d;box-shadow:0 0 0 4px rgba(6,109,77,0.15), inset 0 2px 8px rgba(0,0,0,0.05);
background:#ffffff;transform:translateY(-3px);
}
textarea{height:120px;resize:none;overflow:hidden;line-height:1.7;font-size:17px;}

/* تم إزالة أزرار التعبئة الذكية تحت الحقول */
.auto-buttons{display:none;}

.form-row{
display:grid;grid-template-columns:1fr 1fr;gap:20px;
}

/* تلميحات للأزرار */
button[title] {
position: relative;
}
button[title]:hover::after {
content: attr(title);
position: absolute;
bottom: calc(100% + 10px);
right: 50%;
transform: translateX(50%);
background: rgba(4, 58, 42, 0.95);
color: white;
padding: 10px 15px;
border-radius: 8px;
font-size: 12px;
white-space: pre-line;
z-index: 1000;
border: 1px solid #044a35;
box-shadow: 0 5px 15px rgba(0,0,0,0.15);
max-width: 300px;
min-width: 200px;
}
button[title]:hover::before {
content: '';
position: absolute;
bottom: calc(100% + 2px);
right: 50%;
transform: translateX(50%);
border: 6px solid transparent;
border-top-color: rgba(4, 58, 42, 0.95);
z-index: 1000;
}

/* إشعارات */
.notification {
position: fixed;
top: 100px;
right: 10px;
left: 10px;
background: linear-gradient(135deg, #066d4d 0%, #044a35 100%);
color: white;
padding: 12px 18px;
border-radius: 10px;
box-shadow: 0 6px 20px rgba(4, 74, 53, 0.3);
z-index: 1000;
display: flex;
align-items: center;
gap: 10px;
font-weight: 600;
transform: translateX(150%);
transition: transform 0.4s cubic-bezier(0.68, -0.55, 0.27, 1.55);
border-right: 5px solid #ffd166;
text-align:center;
justify-content:center;
}
.notification.show {
transform: translateX(0);
}
.notification i {
font-size: 18px;
}

/* نافذة المساعدة */
.help-notification {
position: fixed;
bottom: 20px;
right: 20px;
left: auto;
max-width: 300px;
background: linear-gradient(135deg, #5a67d8 0%, #4c51bf 100%);
z-index: 2000;
display: none;
}

/* نافذة الدعم الفني */
.support-modal {
display: none;
position: fixed;
top: 0;
left: 0;
right: 0;
bottom: 0;
background-color: rgba(0, 0, 0, 0.7);
z-index: 1001;
justify-content: center;
align-items: center;
padding: 15px;
}

.support-content {
background: white;
border-radius: 15px;
padding: 25px;
width: 100%;
max-width: 500px;
box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
position: relative;
max-height: 90vh;
overflow-y: auto;
}

.support-header {
display: flex;
justify-content: space-between;
align-items: center;
margin-bottom: 20px;
padding-bottom: 15px;
border-bottom: 2px solid #e0f0ea;
}

.support-header h3 {
color: #044a35;
font-size: 20px;
font-weight: 800;
}

.close-support {
background: none;
border: none;
font-size: 24px;
color: #066d4d;
cursor: pointer;
width: 30px;
height: 30px;
display: flex;
align-items: center;
justify-content: center;
border-radius: 50%;
transition: all 0.3s;
}

.close-support:hover {
background-color: #e8f4f0;
}

.support-form .form-group {
margin-bottom: 20px;
}

.support-actions {
display: flex;
gap: 15px;
margin-top: 25px;
}

.support-action-btn {
flex: 1;
padding: 14px;
border-radius: 10px;
border: none;
color: white;
font-weight: 700;
font-size: 14px;
cursor: pointer;
display: flex;
align-items: center;
justify-content: center;
gap: 10px;
transition: all 0.3s;
box-shadow: 0 4px 10px rgba(0,0,0,0.15);
}

.email-btn {
background: linear-gradient(135deg, #d44646 0%, #b52a2a 100%);
}

.email-btn:hover {
background: linear-gradient(135deg, #b52a2a 0%, #9c1f1f 100%);
transform: translateY(-3px);
}

.whatsapp-support-btn {
background: linear-gradient(135deg, #25D366 0%, #128C7E 100%);
}

.whatsapp-support-btn:hover {
background: linear-gradient(135deg, #128C7E 0%, #075E54 100%);
transform: translateY(-3px);
}

.support-info {
background: #f8fdfa;
border-radius: 10px;
padding: 15px;
margin-top: 20px;
border-right: 4px solid #ffd166;
}

.support-info p {
margin-bottom: 8px;
font-size: 14px;
color: #044a35;
}

.support-info i {
color: #066d4d;
margin-left: 8px;
}

/* أنماط البحث والتصنيف */
#reportSearchContainer {
position: relative;
margin-bottom: 10px;
}

#searchResults {
display: none;
position: absolute;
top: 100%;
left: 0;
right: 0;
background: white;
border: 1px solid #ddd;
border-radius: 6px;
max-height: 200px;
overflow-y: auto;
z-index: 1000;
box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

#searchResults div {
padding: 8px 12px;
cursor: pointer;
border-bottom: 1px solid #eee;
}

#searchResults div:hover {
background-color: #f0f9f6 !important;
color: #066d4d;
}

#searchResults div:last-child {
border-bottom: none;
}

#reportSearch:focus {
outline: none;
border-color: #066d4d;
box-shadow: 0 0 0 2px rgba(6, 109, 77, 0.2);
}

/* قسم الأدوات والوسائل التعليمية في واجهة الإدخال */
.tools-section {
background: #f8fdfa;
padding: 18px;
border-radius: 12px;
border: 1px solid #d4ebe2;
margin-top: 10px;
box-shadow: 0 3px 10px rgba(0,0,0,0.05);
}

.tools-grid {
display: grid;
grid-template-columns: repeat(2, 1fr);
gap: 12px;
}

.tool-checkbox {
display: flex;
align-items: center;
gap: 10px;
padding: 10px;
background: white;
border-radius: 10px;
border: 2px solid #d4ebe2;
transition: all 0.3s;
cursor: pointer;
}

.tool-checkbox:hover {
border-color: #066d4d;
background: #f0f9f6;
transform: translateY(-3px);
box-shadow: 0 4px 8px rgba(6, 109, 77, 0.1);
}

.tool-checkbox input[type="checkbox"] {
width: 20px;
height: 20px;
cursor: pointer;
}

.tool-checkbox span {
font-size: 14px;
font-weight: 700;
color: #083024;
}

.tool-checkbox.checked {
border-color: #066d4d;
background: #e8f4f0;
box-shadow: 0 4px 10px rgba(6, 109, 77, 0.15);
}

/* علامة ✅ في واجهة الإدخال */
.checkmark {
color: #066d4d;
font-size: 16px;
margin-left: 5px;
display: none;
}

.tool-checkbox.checked .checkmark {
display: inline-block;
}

/* خانة عنوان التقرير اليدوية - جديد */
.manual-title-container {
margin-top: 15px;
padding: 15px;
background: #f8fdfa;
border-radius: 12px;
border: 2px solid #d4ebe2;
}

.manual-title-container label {
display: flex;
align-items: center;
gap: 10px;
color: #044a35;
font-weight: 700;
margin-bottom: 10px;
}

.manual-title-container input {
background: white;
border: 2px solid #d4ebe2;
padding: 12px;
border-radius: 8px;
font-size: 16px;
}

/* ==================== تحسينات للهواتف المحمولة ==================== */

/* تحسينات للأجهزة المحمولة العامة */
@media (max-width: 768px) {
.control-bar {
top: 45px;
padding: 8px;
flex-direction: column;
gap: 10px;
height: auto;
min-height: 120px;
}

.header-controls {
width: 100%;
flex-direction: column;
gap: 10px;
}

.date-toggle-container {
width: 100%;
max-width: 100%;
order: 2;
min-width: unset;
}

.app-title {
width: 100%;
max-width: 100%;
font-size: 11px;
padding: 6px 10px;
order: 1;
min-width: unset;
}

.btn-group {
    width: 100%;
    max-width: 100%;
    order: 3;
    margin: 5px auto 0;
}

.btn-row {
    gap: 6px;
}

.btn-row button.main-btn {
    min-height: 55px;
    padding: 8px 6px;
}

.btn-icon {
    font-size: 16px !important;
}

.btn-text {
    font-size: 10px !important;
}

.input-section {
margin-top: 130px;
padding: 15px;
border-radius: 15px;
}

.input-section h2 {
font-size: 20px;
margin-bottom: 20px;
}

.form-row {
grid-template-columns: 1fr;
gap: 15px;
}

.tools-grid {
grid-template-columns: 1fr;
}

.notification {
top: 110px;
padding: 10px 15px;
font-size: 14px;
}

.support-content {
padding: 20px;
max-height: 85vh;
}

.support-actions {
flex-direction: column;
}

.support-action-btn {
width: 100%;
}

#searchResults {
max-height: 150px;
font-size: 13px;
}

.help-notification {
right: 10px;
left: 10px;
max-width: unset;
bottom: 10px;
}
}

/* تحسينات للشاشات الصغيرة جداً */
@media (max-width: 480px) {
.top-marquee {
font-size: 11px;
padding: 8px 5px;
height: 40px;
}

.marquee-inner {
animation-duration: 35s;
}

.control-bar {
min-height: 110px;
}

.app-title {
font-size: 10px;
padding: 5px 8px;
}

.date-toggle-container {
font-size: 11px;
padding: 5px 8px;
}

.btn-group {
    margin-top: 3px;
}

.btn-row {
    gap: 4px;
}

.btn-row button.main-btn {
    min-height: 50px;
    padding: 6px 4px;
}

.btn-icon {
    font-size: 14px !important;
    margin-bottom: 3px;
}

.btn-text {
    font-size: 9px !important;
}

.input-section {
margin-top: 125px;
padding: 12px;
}

input, select, textarea {
padding: 12px;
font-size: 16px;
}

.form-group label {
font-size: 13px;
}

.form-group label i {
font-size: 13px;
padding: 5px;
}

.auto-btn {
padding: 10px;
font-size: 12px;
}

.tool-checkbox {
padding: 6px;
}

.tool-checkbox span {
font-size: 12px;
}

.support-header h3 {
font-size: 18px;
}

.support-action-btn {
padding: 12px;
font-size: 13px;
}
}

/* تحسينات للأجهزة فائقة الصغر */
@media (max-width: 360px) {
.control-bar {
min-height: 105px;
padding: 6px;
}

.btn-row {
    gap: 3px;
}

.btn-row button.main-btn {
    min-height: 45px;
    padding: 5px 3px;
}

.btn-icon {
    font-size: 12px !important;
}

.btn-text {
    font-size: 8px !important;
}

.input-section {
margin-top: 120px;
padding: 10px;
}

input, select, textarea {
padding: 10px;
}
}

/* تحسينات خاصة لـ iPhone ذات الشقوق */
/* iPhone X/XS/11 Pro/12 mini/13 mini */
@media only screen and (device-width: 375px) and (device-height: 812px) and (-webkit-device-pixel-ratio: 3) {
.top-marquee {
padding-top: env(safe-area-inset-top);
padding-bottom: env(safe-area-inset-top);
height: calc(45px + env(safe-area-inset-top) * 2);
}

.control-bar {
top: calc(45px + env(safe-area-inset-top));
padding-top: env(safe-area-inset-top);
padding-bottom: env(safe-area-inset-top);
}

.input-section {
margin-top: calc(140px + env(safe-area-inset-top) * 2);
}
}

/* iPhone 12/13/14 */
@media only screen and (device-width: 390px) and (device-height: 844px) and (-webkit-device-pixel-ratio: 3) {
.top-marquee {
padding-top: env(safe-area-inset-top);
padding-bottom: env(safe-area-inset-top);
height: calc(45px + env(safe-area-inset-top) * 2);
}

.control-bar {
top: calc(45px + env(safe-area-inset-top));
padding-top: env(safe-area-inset-top);
padding-bottom: env(safe-area-inset-top);
}

.input-section {
margin-top: calc(140px + env(safe-area-inset-top) * 2);
}
}

/* iPhone 12/13/14 Pro Max */
@media only screen and (device-width: 428px) and (device-height: 926px) and (-webkit-device-pixel-ratio: 3) {
.top-marquee {
padding-top: env(safe-area-inset-top);
padding-bottom: env(safe-area-inset-top);
height: calc(45px + env(safe-area-inset-top) * 2);
}

.control-bar {
top: calc(45px + env(safe-area-inset-top));
padding-top: env(safe-area-inset-top);
padding-bottom: env(safe-area-inset-top);
}

.input-section {
margin-top: calc(140px + env(safe-area-inset-top) * 2);
}
}

/* تحسينات للشاشات الطويلة */
@media (max-height: 700px) and (orientation: portrait) {
.input-section {
margin-top: 110px;
padding: 10px;
}

.form-group {
margin-bottom: 15px;
}

.form-group label {
font-size: 13px;
margin-bottom: 5px;
}

input, select, textarea {
padding: 10px;
font-size: 14px;
}

.auto-btn {
padding: 8px;
font-size: 12px;
}
}

/* تحسينات للوضع الأفقي (Landscape) */
@media (max-width: 850px) and (orientation: landscape) {
.control-bar {
flex-direction: row;
height: 70px;
}

.header-controls {
flex-direction: row;
gap: 10px;
}

.app-title {
max-width: 200px;
font-size: 10px;
padding: 4px 8px;
min-width: 180px;
}

.date-toggle-container {
max-width: 180px;
min-width: 150px;
}

.btn-group {
    max-width: 500px;
}

.btn-row button.main-btn {
    min-height: 50px;
}

.input-section {
margin-top: 90px;
padding: 12px;
}

.form-row {
grid-template-columns: 1fr 1fr;
gap: 10px;
}
}

/* منع التكبير التلقائي على iOS عند التركيز على الحقول */
input, select, textarea {
font-size: 16px !important;
}

/* إصلاح مشكلة التكبير في iOS */
@media screen and (max-width: 767px) {
input, select, textarea, .form-group {
font-size: 16px !important;
}
}

/* تحسين تجربة اللمس */
button, .auto-btn, .tool-checkbox {
-webkit-touch-callout: none;
-webkit-user-select: none;
user-select: none;
}

/* تحسين التمرير السلس */
.input-section, .support-content, #searchResults {
-webkit-overflow-scrolling: touch;
}

/* ==================== قسم PDF المعدل ==================== */
@page{
  size:A4;
  margin:10mm;
}

:root{
  --main:#062f25;
  --border:#2f9e8f;
}

#report-content{
  width:100%;
  max-width:210mm;
  margin:4mm auto 0 auto;
  padding:0 6mm;
  box-sizing:border-box;
  display:none;
  font-family:'Cairo',sans-serif;
  background:#fff;
}

/* الهيدر - تم التعديل هنا */
.header{
  background:var(--main);
  height:150px;
  border-radius:8px;
  color:#fff;
  display:flex;
  align-items:center;
  justify-content:center;
  position:relative;
  margin-bottom:8px;
}
.header img{width:140px;}
.header-school-title{
  position:absolute;
  right:12px;
  top:20px;
  font-size:14px; /* تم تكبير الخط 3 درجات من 11px إلى 14px */
  font-weight:800;
}
.header-school{
  position:absolute;
  right:12px;
  top:45px; /* تم تعديل الموقع ليصبح تحت العنوان */
  font-size:18px; /* تم تكبير الخط 3 درجات من 14px إلى 18px */
  font-weight:900;
}
.header-education{
  position:absolute;
  left:50%;
  bottom:18px;
  transform:translateX(-50%);
  font-size:16px; /* تم تكبير الخط 3 درجات من 13px إلى 16px */
  font-weight:800;
  text-align:center;
  width:100%;
}
.header-date{
  position:absolute;
  left:12px;
  top:10px;
  font-size:12px; /* تم تكبير الخط 3 درجات من 9px إلى 12px */
  text-align:right;
}

/* مربعات المعلومات - تم التعديل هنا حسب طلبك: تبديل نوع التقرير والمستهدفون */
.info-grid{
  display:grid;
  grid-template-columns:repeat(4,1fr);
  gap:6px;
  margin-bottom:6px;
}
.info-grid2{
  display:grid;
  grid-template-columns:repeat(3,1fr);
  gap:6px;
  margin-bottom:6px;
}

.info-box{
  border:1px solid var(--border);
  border-radius:7px;
  padding:14px 4px 6px;
  position:relative;
  text-align:center;
  font-size:11px; /* تم تصغير الخط للمحتوى */
  min-height:34px;
  overflow:hidden;
}
.info-title{
  position:absolute;
  top:4px;
  right:50%;
  transform:translateX(50%);
  font-size:8px;
  font-weight:800;
  color:var(--main);
  white-space:nowrap;
}
.info-value{
  font-size:11px; /* تم تصغير خط المحتوى */
  font-weight:600;
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap;
}

/* التعديل الخاص: تصغير محتوى نوع التقرير فقط */
#reportTypeBox{
  font-size: 9px;     /* تصغير المحتوى */
  font-weight: 600;
}

/* مادة | درس - تم التعديل هنا */
.subject-lesson-box{
  border:1px solid var(--border);
  border-radius:7px;
  position:relative;
  padding:14px 4px 6px;
  overflow:hidden;
  height: 48px;          /* ارتفاع ثابت */
  min-height: 48px;
  max-height: 48px;
}
.subject-lesson-title{
  position:absolute;
  top:4px;
  right:50%;
  transform:translateX(50%);
  font-size:8px;
  font-weight:800;
  color:var(--main);
}
.subject-lesson{
  display:grid;
  grid-template-columns:1fr 1px 1fr;
  align-items:center;
  text-align:center;
  font-size:11px; /* تم تصغير خط المحتوى */
  height: 100%;
}
.subject-divider{
  background:var(--border);
  height:60%;
  margin:auto;
}

/* الهدف التربوي */
.box-objective{
  border:1px solid var(--border);
  border-radius:8px;
  padding:8px;
  margin-bottom:6px;
  height:95px;
  display:flex;
  flex-direction:column;
  overflow:hidden;
}
.box-objective .box-title{
  text-align:center;
  color:var(--main);
  font-weight:800;
  font-size:8px;
  margin-bottom:4px;
}
.box-objective .box-content{
  font-size:14px; /* تم تكبير الخط 3 درجات من 11px إلى 14px */
  line-height:1.5;
  text-align:center;
  overflow:hidden;
}

/* التعديل الخاص: تكبير عنوان الهدف التربوي فقط */
.box-objective .box-title{
  font-size: 17px;      /* كبّر العنوان */
  font-weight: 900;
}

/* المربعات الكبيرة */
.box{
  border:1px solid var(--border);
  border-radius:8px;
  padding:8px;
  margin-bottom:6px;
  height:137px;
  display:flex;
  flex-direction:column;
  overflow:hidden;
}
.box-title{
  text-align:center;
  color:var(--main);
  font-weight:800;
  font-size:14px; /* تم تكبير الخط 3 درجات من 11px إلى 14px */
  margin-bottom:4px;
}
.box-content{
  font-size:14px; /* تم تكبير الخط 3 درجات من 11px إلى 14px */
  line-height:1.5;
  text-align:center;
  overflow:hidden;
}

/* الصفوف */
.row{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:6px;
}

/* الأدوات */
.tools-box{
  border:1px solid var(--border);
  border-radius:8px;
  padding:6px;
  margin-bottom:6px;
  overflow:hidden;
}
.tools-title{
  text-align:center;
  font-weight:800;
  color:var(--main);
  font-size:13px; /* تم تكبير الخط 3 درجات من 10px إلى 13px */
  margin-bottom:4px;
}
.tools-list{
  display:flex;
  flex-wrap:wrap;
  justify-content:center;
  gap:6px;
  font-size:12px; /* تم تكبير الخط 3 درجات من 9px إلى 12px */
}
.tool{
  background:#eef7f4;
  border:1px solid #cfe8df;
  border-radius:16px;
  padding:3px 8px;
  display:flex;
  align-items:center;
  gap:5px;
  white-space:nowrap;
}
.tool span{
  background:var(--border);
  color:#fff;
  border-radius:50%;
  width:12px;
  height:12px;
  display:flex;
  align-items:center;
  justify-content:center;
  font-size:8px;
}

/* ========== الصور - إعدادات جديدة جذرية ========== */
.images{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:6px;
  margin-bottom:6px;
}
.image-box{
  border:1px dashed var(--border);
  height:160px;
  display:flex;
  align-items:center;
  justify-content:center;
  overflow:hidden;
  background:#f9fcfb;
  position:relative;
}
.image-box::before{
  content:'صورة توثيقية';
  position:absolute;
  top:4px;
  right:4px;
  font-size:12px; /* تم تكبير الخط 3 درجات من 9px إلى 12px */
  background:rgba(255,255,255,.9);
  padding:1px 5px;
  border-radius:3px;
  z-index:1;
}

.image-box img{
  width: 65%;
  height: 100%;
  object-fit: contain;
  display: block;
}

/* التوقيعات */
.signatures{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:30px;
  text-align:center;
  font-size:13px; /* تم تكبير الخط 3 درجات من 10px إلى 13px */
  margin-bottom:6px;
}
.signature-box{
  padding-top:4px;
}
.signature-role{
  font-size:12px; /* تم تكبير الخط 3 درجات من 9px إلى 12px */
  color:var(--main);
  font-weight:700;
  margin-bottom:2px;
}
.signature-name{
  font-size:14px; /* تم تكبير الخط 3 درجات من 11px إلى 14px */
  font-weight:900;
  color:#000;
}
.sign-line{
  border-top:1px solid #000;
  margin:6px auto 0;
  width:70%;
}

/* الفوتر */
.footer-box{
  background:var(--main);
  color:#fff;
  text-align:center;
  font-size:11px; /* تم تكبير الخط 3 درجات من 8px إلى 11px */
  padding:3px 4px;
  border-radius:6px;
}

/* لضمان ظهور الألوان في PDF */
.pdf-export * {
    -webkit-print-color-adjust: exact !important;
    print-color-adjust: exact !important;
    color-adjust: exact !important;
}

/* إصلاحات للأندرويد */
.android-fix * {
    -webkit-tap-highlight-color: rgba(0,0,0,0);
}

/* إصلاحات الشريط السفلي على iOS */
@supports (padding-bottom: env(safe-area-inset-bottom)) {
    .wrapper {
        padding-bottom: env(safe-area-inset-bottom);
    }
}

/* مؤشر التحميل للذكاء الاصطناعي */
.ai-loading-indicator {
    display: none;
    position: absolute;
    top: -5px;
    right: -5px;
    width: 20px;
    height: 20px;
    background: #ffd166;
    border-radius: 50%;
    animation: pulse 1.5s infinite;
    z-index: 10;
}

@keyframes pulse {
    0% { transform: scale(0.8); opacity: 0.7; }
    50% { transform: scale(1.2); opacity: 1; }
    100% { transform: scale(0.8); opacity: 0.7; }
}

/* إدخال التاريخ يدوياً */
#manualDateInput {
    width: 100%;
    padding: 6px;
    border: 1px solid #ccc;
    border-radius: 6px;
    font-size: 12px;
    text-align: center;
    font-family: 'Cairo', sans-serif;
}

#manualDateInput:focus {
    outline: none;
    border-color: #066d4d;
    box-shadow: 0 0 0 2px rgba(6, 109, 77, 0.2);
}

/* منع النص من دفع المربع للتمدد - تم تعديلها */
#subjectBox,
#lessonBox{
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  width: 100%;
  height: 100%;
  padding: 0 5px;
  transition: all 0.3s ease;
}
</style>
</head>

<body>

<div class="top-marquee">
<div class="marquee-inner">
<i class="fas fa-bullhorn" style="margin-left:10px;"></i>
اختر نوع التقرير من التصنيفات المتاحة وحدّد الأدوات التعليمية المستخدمة في الدرس،
ثم اضغط زر التعبئة لتوليد محتوى البنود تلقائيًا.
يمكن إعادة التوليد لتغيير الصياغة وتعديل النصوص عند الحاجة،
كما يمكن إدخال اسم أي تقرير غير موجود ليتم توليده مباشرة وبسهولة عبر استخدام زر التعبئة.
</div>
</div>

<div class="control-bar">
    <div class="header-controls">
        <!-- العنوان بدون مفتاح Gemini - توسيط في المنتصف -->
        <div class="app-title" onclick="handleTeacherTitleClick()">
            <i class="fas fa-user-tie"></i>
            <span>تنفيذ: المعلم فهد الخالدي</span>
        </div>
        
        <!-- حاوية التاريخ الجديدة - إدخال يدوي -->
        <div class="date-toggle-container">
            <div class="date-display" id="currentDateDisplay">
                التاريخ الحالي
            </div>
            <input type="text" id="manualDateInput" 
                   placeholder="أدخل التاريخ الهجري (مثال: ١٤٤٦/٠٦/١٥)" 
                   onchange="updateManualDate()">
        </div>
        
        <!-- مجموعة الأزرار المعدلة والمرتبة في صفين -->
        <div class="btn-group">
            <!-- الصف الأول: 3 أزرار -->
            <div class="btn-row">
                <button class="main-btn" id="saveTeacherBtn" onclick="saveTeacherData()" title="حفظ بيانات إدارة التعليم، اسم المدرسة، الصف، المادة، المستهدفون، المكان">
                    <i class="fas fa-chalkboard-teacher btn-icon"></i>
                    <span class="btn-text">حفظ بيانات المعلم</span>
                </button>
                <button class="main-btn" id="aiFillBtn" onclick="fillWithAI()" title="تعبئة جميع الحقول باستخدام الذكاء الاصطناعي">
                    <i class="fas fa-robot btn-icon"></i>
                    <span class="btn-text">تعبئة ذكية</span>
                </button>
                <button class="main-btn" id="pdfBtn" onclick="downloadPDF()" title="تحويل التقرير إلى PDF وتنزيله">
                    <i class="fas fa-file-pdf btn-icon"></i>
                    <span class="btn-text">تنزيل PDF</span>
                </button>
            </div>
            
            <!-- الصف الثاني: 4 أزرار (تمت إضافة زر التعبئة الذكية ٢) -->
            <div class="btn-row">
                <button class="main-btn" id="aiFillBtn2" onclick="fillWithAI2()" title="تعبئة ذكية متقدمة باستخدام مفاتيح جيمني المتعددة">
                    <i class="fas fa-magic btn-icon"></i>
                    <span class="btn-text">تعبئة ذكية ٢</span>
                </button>
                <button class="main-btn" id="clearBtn" onclick="clearData()" title="مسح جميع البيانات المدخلة">
                    <i class="fas fa-trash-alt btn-icon"></i>
                    <span class="btn-text">مسح البيانات</span>
                </button>
                <button class="main-btn" id="supportBtn" onclick="openSupportModal()" title="الدعم الفني والتواصل مع المطور">
                    <i class="fas fa-headset btn-icon"></i>
                    <span class="btn-text">الدعم الفني</span>
                </button>
                <button class="main-btn" id="whatsappBtn" onclick="sharePDFWhatsApp()" title="مشاركة التقرير عبر واتساب">
                    <i class="fab fa-whatsapp btn-icon"></i>
                    <span class="btn-text">مشاركة واتساب</span>
                </button>
            </div>
        </div>
    </div>
</div>

<!-- نافذة التحكم الإداري المخفية -->
<div class="overlay" id="adminOverlay" onclick="closeAdminPanel()"></div>

<div class="admin-panel" id="adminPanel">
    <div class="admin-header">
        <h3><i class="fas fa-key"></i> لوحة التحكم الإداري - مفاتيح Gemini</h3>
        <button class="close-admin" onclick="closeAdminPanel()">×</button>
    </div>
    
    <div class="admin-content">
        <!-- مفتاح 1 -->
        <div class="key-field">
            <div class="key-header">
                <label><i class="fas fa-key"></i> المفتاح 1</label>
                <div class="key-actions">
                    <button class="check-key-btn" onclick="checkKey(1)">
                        <i class="fas fa-check"></i> تحقق
                    </button>
                    <button class="toggle-key-btn" id="toggleKey1" onclick="toggleKey(1)">
                        <i class="fas fa-power-off"></i> تفعيل
                    </button>
                </div>
            </div>
            <input type="password" class="key-input" id="keyInput1" placeholder="أدخل مفتاح Gemini API الأول">
            <div class="key-status" id="keyStatus1"></div>
        </div>
        
        <!-- مفتاح 2 -->
        <div class="key-field">
            <div class="key-header">
                <label><i class="fas fa-key"></i> المفتاح 2</label>
                <div class="key-actions">
                    <button class="check-key-btn" onclick="checkKey(2)">
                        <i class="fas fa-check"></i> تحقق
                    </button>
                    <button class="toggle-key-btn" id="toggleKey2" onclick="toggleKey(2)">
                        <i class="fas fa-power-off"></i> تفعيل
                    </button>
                </div>
            </div>
            <input type="password" class="key-input" id="keyInput2" placeholder="أدخل مفتاح Gemini API الثاني">
            <div class="key-status" id="keyStatus2"></div>
        </div>
        
        <!-- مفتاح 3 -->
        <div class="key-field">
            <div class="key-header">
                <label><i class="fas fa-key"></i> المفتاح 3</label>
                <div class="key-actions">
                    <button class="check-key-btn" onclick="checkKey(3)">
                        <i class="fas fa-check"></i> تحقق
                    </button>
                    <button class="toggle-key-btn" id="toggleKey3" onclick="toggleKey(3)">
                        <i class="fas fa-power-off"></i> تفعيل
                    </button>
                </div>
            </div>
            <input type="password" class="key-input" id="keyInput3" placeholder="أدخل مفتاح Gemini API الثالث">
            <div class="key-status" id="keyStatus3"></div>
        </div>
        
        <!-- مفتاح 4 -->
        <div class="key-field">
            <div class="key-header">
                <label><i class="fas fa-key"></i> المفتاح 4</label>
                <div class="key-actions">
                    <button class="check-key-btn" onclick="checkKey(4)">
                        <i class="fas fa-check"></i> تحقق
                    </button>
                    <button class="toggle-key-btn" id="toggleKey4" onclick="toggleKey(4)">
                        <i class="fas fa-power-off"></i> تفعيل
                    </button>
                </div>
            </div>
            <input type="password" class="key-input" id="keyInput4" placeholder="أدخل مفتاح Gemini API الرابع">
            <div class="key-status" id="keyStatus4"></div>
        </div>
        
        <!-- إجراءات عامة للمفاتيح -->
        <div class="key-actions-global">
            <button class="global-action-btn activate-all-btn" onclick="activateAllKeys()">
                <i class="fas fa-play"></i> تفعيل الكل
            </button>
            <button class="global-action-btn deactivate-all-btn" onclick="deactivateAllKeys()">
                <i class="fas fa-stop"></i> إيقاف الكل
            </button>
            <button class="global-action-btn save-keys-btn" onclick="saveKeys()">
                <i class="fas fa-save"></i> حفظ المفاتيح
            </button>
        </div>
        
        <div class="admin-info" style="background: #f8fdfa; padding: 15px; border-radius: 10px; margin-top: 20px; border-right: 4px solid #ffd166;">
            <p style="font-size: 12px; color: #044a35; margin-bottom: 8px;">
                <i class="fas fa-info-circle"></i> النظام سيتحول تلقائيًا بين المفاتيح عند تجاوز الحد اليومي
            </p>
            <p style="font-size: 12px; color: #044a35;">
                <i class="fas fa-sync-alt"></i> زر "التعبئة الذكية ٢" سيستخدم المفاتيح المفعّلة بالتناوب
            </p>
        </div>
    </div>
</div>

<!-- إشعارات -->
<div class="notification" id="saveNotification">
<i class="fas fa-check-circle"></i>
<span>تم حفظ بيانات المعلم بنجاح!</span>
</div>

<!-- نافذة الدعم الفني -->
<div class="support-modal" id="supportModal">
<div class="support-content">
<div class="support-header">
<h3><i class="fas fa-headset" style="margin-left:10px;"></i>الدعم الفني</h3>
<button class="close-support" onclick="closeSupportModal()">×</button>
</div>

<div class="support-form">
<div class="form-group">
<label for="supportName"><i class="fas fa-user"></i>الاسم الكامل</label>
<input type="text" id="supportName" placeholder="أدخل اسمك الكامل">
</div>

<div class="form-group">
<label for="supportPhone"><i class="fas fa-phone"></i>رقم التواصل</label>
<input type="tel" id="supportPhone" placeholder="أدخل رقم الجوال أو الهاتف">
</div>

<div class="form-group">
<label for="supportIssue"><i class="fas fa-exclamation-circle"></i>تفاصيل المشكلة</label>
<textarea id="supportIssue" rows="4" placeholder="صف مشكلتك بالتفصيل..."></textarea>
</div>

<div class="support-info">
<p><i class="fas fa-envelope"></i>البريد الإلكتروني: iFahadenglish@gmail.com</p>
<p><i class="fab fa-whatsapp"></i>واتساب: +966597077245</p>
</div>

<div class="support-actions">
<button class="support-action-btn email-btn" onclick="sendEmailSupport()">
<i class="fas fa-envelope"></i>مراسلة عبر البريد
</button>
<button class="support-action-btn whatsapp-support-btn" onclick="sendWhatsAppSupport()">
<i class="fab fa-whatsapp"></i>مراسلة عبر واتساب
</button>
</div>
</div>
</div>
</div>

<div class="wrapper">
<div class="input-section">
  
  <h2><i class="fas fa-tools" style="margin-left:10px;"></i>أداة إصدار التقارير التربوية</h2>
  
  <div class="form-group">
    <label><i class="fas fa-file-alt"></i>اسم التقرير</label>
    
    <!-- التصنيف العام -->
    <select id="reportCategory" oninput="handleReportCategory()" style="margin-bottom:10px;">
        <option value="">اختر تصنيف التقرير</option>
        <option value="التقارير التعليمية الصفية">أولا: التقارير التعليمية الصفية</option>
        <option value="التقارير العلاجية والدعم الفردي">ثانيا: التقارير العلاجية والدعم الفردي</option>
        <option value="التقارير التحفيزية والسلوكية">ثالثا: التقارير التحفيزية والسلوكية</option>
        <option value="تقارير الأنشطة غير الصفية">رابعا: تقارير الأنشطة غير الصفية</option>
        <option value="تقارير التواصل مع أولياء الأمور والمجتمع">سادسا: تقارير التواصل مع أولياء الأمور والمجتمع</option>
        <option value="التقارير التخطيطية والتنظيمية">سادسا: التقارير التخطيطية والتنظيمية</option>
        <option value="تقارير التقييم والمتابعة">سابعا: تقارير التقييم والمتابعة</option>
        <option value="تقارير التدريب والتطوير المهني">ثامنا: تقارير التدريب والتطوير المهني</option>
        <option value="تقارير توظيف التكنولوجيا">تاسعا: تقارير توظيف التكنولوجيا</option>
        <option value="تقارير البحث والتطوير المناهجي">عاشرا: تقارير البحث والتطوير المناهجي</option>
        <option value="تقارير الجودة واللجان">حادي عشر: تقارير الجودة واللجان</option>
        <option value="تقارير الأمن والسلامة">ثاني عشر: تقارير الأمن والسلامة</option>
        <option value="أخرى">تقارير أخرى (إدخال يدوي)</option>
    </select>
    
    <!-- حقل البحث -->
    <div id="reportSearchContainer" style="display:block; margin-bottom:10px; position:relative;">
        <input type="text" id="reportSearch" placeholder="ابحث عن تقرير..." style="width:100%; padding:12px; border:1px solid #d4ebe2; border-radius:6px; font-size:14px;">
        <div id="searchResults" style="display:none; position:absolute; top:100%; left:0; right:0; background:white; border:1px solid #ddd; border-radius:6px; max-height:200px; overflow-y:auto; z-index:1000; box-shadow:0 4px 12px rgba(0,0,0,0.1);"></div>
    </div>
    
    <!-- قائمة التقارير المنسدلة -->
    <select id="reportType" oninput="handleReportType()" style="display:none;">
        <option value="">اختر تقريرًا</option>
    </select>
    
    <!-- حقل الإدخال للنوع "أخرى" -->
    <input id="reportTypeInput" placeholder="أدخل اسم التقرير" oninput="updateReport()" style="display:none; margin-top:8px;">
    
    <!-- خانة عنوان التقرير اليدوية - جديد -->
    <div class="manual-title-container">
        <label><i class="fas fa-heading"></i>عنوان التقرير (يدوي)</label>
        <input type="text" id="manualReportTitle" placeholder="أدخل عنوان التقرير يدوياً..." oninput="updateManualTitle()">
    </div>
  </div>
  
  <div class="form-group">
    <label><i class="fas fa-university"></i>إدارة التعليم</label>
    <select id="education" oninput="updateReport()">
      <option>الإدارة العامة للتعليم بمنطقة مكة المكرمة</option>
      <option>الإدارة العامة للتعليم بمنطقة الرياض</option>
      <option>الإدارة العامة للتعليم بمنطقة المدينة المنورة</option>
      <option>الإدارة العامة للتعليم بالمنطقة الشرقية</option>
      <option>الإدارة العامة للتعليم بمنطقة القصيم</option>
      <option>الإدارة العامة للتعليم بمنطقة عسير</option>
      <option>الإدارة العامة للتعليم بمنطقة تبوك</option>
      <option>الإدارة العامة للتعليم بمنطقة حائل</option>
      <option>الإدارة العامة للتعليم بمنطقة الحدود الشمالية</option>
      <option>الإدارة العامة للتعليم بمنطقة جازان</option>
      <option>الإدارة العامة للتعليم بمنطقة نجران</option>
      <option>الإدارة العامة للتعليم بمنطقة الباحة</option>
      <option>الإدارة العامة للتعليم بمنطقة الجوف</option>
      <option>الإدارة العامة للتعليم بمحافظة الأحساء</option>
      <option>الإدارة العامة للتعليم بمحافظة الطائف</option>
      <option>الإدارة العامة للتعليم بمحافظة جدة</option>
    </select>
  </div>
  
  <div class="form-group">
    <label><i class="fas fa-school"></i>اسم المدرسة</label>
    <input id="school" value="سعيد بن العاص" placeholder="أدخل اسم المدرسة" oninput="updateReport()">
  </div>
  
  <div class="form-row">
    <div class="form-group">
      <label><i class="fas fa-chalkboard-teacher"></i>صفة المعلّم</label>
      <select id="teacherType" oninput="updateReport()">
        <option selected>المعلم</option>
        <option>المعلمة</option>
      </select>
    </div>
    
    <div class="form-group">
      <label><i class="fas fa-user"></i>اسم المعلّم</label>
      <input id="teacher" value="فهد الخالدي" placeholder="اسم المعلم" oninput="updateReport()">
    </div>
  </div>
  
  <div class="form-row">
    <div class="form-group">
      <label><i class="fas fa-user-tie"></i>صفة المدير</label>
      <select id="principalType" oninput="updateReport()">
        <option selected>المدير</option>
        <option>المديرة</option>
      </select>
    </div>
    
    <div class="form-group">
      <label><i class="fas fa-user-cog"></i>اسم المدير</label>
      <input id="principal" value="نايف اللحياني" placeholder="اسم مدير المدرسة" oninput="updateReport()">
    </div>
  </div>
  
  <div class="form-row">
    <div class="form-group">
      <label><i class="fas fa-users-class"></i>الصف</label>
      <input id="grade" placeholder="مثال: ٥/٣" oninput="updateReport()">
    </div>
    
    <div class="form-group">
      <label><i class="fas fa-calendar-alt"></i>الفصل الدراسي</label>
      <select id="term" oninput="updateReport()">
        <option></option><option>الأول</option><option>الثاني</option>
      </select>
    </div>
  </div>
  
  <!-- المادة والدرس - أصبحا بجوار بعضهما -->
  <div class="form-row">
    <div class="form-group">
      <label><i class="fas fa-book"></i>المادة</label>
      <input id="subject" placeholder="مثال: لغتي – علوم – رياضيات" oninput="updateReport()">
    </div>
    
    <div class="form-group">
      <label><i class="fas fa-book-open"></i>الدرس</label>
      <input id="lesson" placeholder="مثال: درس الضرب - درس النباتات" oninput="updateReport()">
    </div>
  </div>
  
  <div class="form-row">
    <div class="form-group">
      <label><i class="fas fa-bullseye"></i>المستهدفون</label>
      <input id="target" placeholder="مثال: جميع طلاب الصف" oninput="updateReport()">
    </div>
    
    <div class="form-group">
      <label><i class="fas fa-user-check"></i>عدد الحضور</label>
      <input id="count" placeholder="مثال: ٢٥ طالب" oninput="updateReport()">
    </div>
  </div>
  
  <div class="form-group">
    <label><i class="fas fa-map-marker-alt"></i>مكان التنفيذ</label>
    <input id="place" placeholder="مثال: داخل الصف – المختبر" oninput="updateReport()">
  </div>
  
  <div class="form-group">
    <label><i class="fas fa-flag"></i>الهدف التربوي</label>
    <textarea id="goal" placeholder="أدخل الهدف التربوي" oninput="updateReport()"></textarea>
    <!-- تم إزالة أزرار التعبئة الذكية -->
  </div>
  
  <div class="form-group">
    <label><i class="fas fa-file-signature"></i>نبذة مختصرة</label>
    <textarea id="summary" placeholder="أدخل نبذة مختصرة" oninput="updateReport()"></textarea>
    <!-- تم إزالة أزرار التعبئة الذكية -->
  </div>
  
  <div class="form-group">
    <label><i class="fas fa-tasks"></i>إجراءات التنفيذ</label>
    <textarea id="steps" placeholder="كيف تم تنفيذ النشاط؟" oninput="updateReport()"></textarea>
    <!-- تم إزالة أزرار التعبئة الذكية -->
  </div>
  
  <div class="form-group">
    <label><i class="fas fa-chess-board"></i>الاستراتيجيات</label>
    <textarea id="strategies" placeholder="ما هي الاستراتيجيات" oninput="updateReport()"></textarea>
    <!-- تم إزالة أزرار التعبئة الذكية -->
  </div>
  
  <div class="form-row">
    <div class="form-group">
      <label><i class="fas fa-thumbs-up"></i>نقاط القوة</label>
      <textarea id="strengths" placeholder="نقاط القوة" oninput="updateReport()"></textarea>
      <!-- تم إزالة أزرار التعبئة الذكية -->
    </div>
    
    <div class="form-group">
      <label><i class="fas fa-tools"></i>نقاط التحسين</label>
      <textarea id="improve" placeholder="نقاط تحتاج تطوير" oninput="updateReport()"></textarea>
      <!-- تم إزالة أزرار التعبئة الذكية -->
    </div>
  </div>
  
  <div class="form-group">
    <label><i class="fas fa-lightbulb"></i>التوصيات</label>
    <textarea id="recomm" placeholder="توصيات مستقبلية" oninput="updateReport()"></textarea>
    <!-- تم إزالة أزرار التعبئة الذكية -->
  </div>
  
  <!-- قسم الأدوات والوسائل التعليمية -->
  <div class="form-group">
    <label><i class="fas fa-tools"></i>الأدوات والوسائل التعليمية</label>
    <div class="tools-section">
      <div class="tools-grid">
        <label class="tool-checkbox" onclick="toggleTool(this)">
          <input type="checkbox" id="tool1" value="سبورة" style="display:none;">
          <span>سبورة</span>
          <span class="checkmark">✅</span>
        </label>
        <label class="tool-checkbox" onclick="toggleTool(this)">
          <input type="checkbox" id="tool2" value="سبورة ذكية" style="display:none;">
          <span>سبورة ذكية</span>
          <span class="checkmark">✅</span>
        </label>
        <label class="tool-checkbox" onclick="toggleTool(this)">
          <input type="checkbox" id="tool3" value="جهاز عرض" style="display:none;">
          <span>جهاز عرض</span>
          <span class="checkmark">✅</span>
        </label>
        <label class="tool-checkbox" onclick="toggleTool(this)">
          <input type="checkbox" id="tool4" value="أوراق عمل" style="display:none;">
          <span>أوراق عمل</span>
          <span class="checkmark">✅</span>
        </label>
        <label class="tool-checkbox" onclick="toggleTool(this)">
          <input type="checkbox" id="tool5" value="حاسب" style="display:none;">
          <span>حاسب</span>
          <span class="checkmark">✅</span>
        </label>
        <label class="tool-checkbox" onclick="toggleTool(this)">
          <input type="checkbox" id="tool6" value="عرض تقديمي" style="display:none;">
          <span>عرض تقديمي</span>
          <span class="checkmark">✅</span>
        </label>
        <label class="tool-checkbox" onclick="toggleTool(this)">
          <input type="checkbox" id="tool7" value="بطاقات تعليمية" style="display:none;">
          <span>بطاقات تعليمية</span>
          <span class="checkmark">✅</span>
        </label>
        <label class="tool-checkbox" onclick="toggleTool(this)">
          <input type="checkbox" id="tool8" value="صور توضيحية" style="display:none;">
          <span>صور توضيحية</span>
          <span class="checkmark">✅</span>
        </label>
        <label class="tool-checkbox" onclick="toggleTool(this)">
          <input type="checkbox" id="tool9" value="كتاب" style="display:none;">
          <span>كتاب</span>
          <span class="checkmark">✅</span>
        </label>
        <label class="tool-checkbox" onclick="toggleTool(this)">
          <input type="checkbox" id="tool10" value="أدوات رياضية" style="display:none;">
          <span>أدوات رياضية</span>
          <span class="checkmark">✅</span>
        </label>
      </div>
      <div style="text-align:center; margin-top:10px; font-size:11px; color:#666;">
        <i class="fas fa-info-circle"></i> اضغط على الأداة لتحديدها، ستظهر علامة ✅ عند التحديد
      </div>
    </div>
  </div>
  
  <div class="form-row">
    <div class="form-group">
      <label><i class="fas fa-camera"></i>الصورة 1</label>
      <input type="file" accept="image/*" placeholder="ارفع صورة" onchange="loadImage(this,'imgBox1')">
    </div>
    
    <div class="form-group">
      <label><i class="fas fa-camera"></i>الصورة 2</label>
      <input type="file" accept="image/*" placeholder="ارفع صورة" onchange="loadImage(this,'imgBox2')">
    </div>
  </div>

</div>
</div>

<!-- قسم PDF المعدل - تم تبديل نوع التقرير والمستهدفون حسب طلبك -->
<div id="report-content" class="pdf-export" style="display:none;">

<div class="header">
  <img src="https://i.ibb.co/1fc5gB6v/9-C92-E57-B-23-FA-479-D-A024-1-D5-F871-B4-F8-D.png">
  <div class="header-school-title">اسم المدرسة</div>
  <div class="header-school" id="schoolBox"></div>
  <div class="header-education" id="educationBox"></div>
  <div class="header-date">
    <span id="hDate"></span><br>
    <span id="gDate"></span>
  </div>
</div>

<!-- تم تبديل المستهدفون مكان نوع التقرير هنا -->
<div class="info-grid">
  <div class="info-box"><div class="info-title">الفصل الدراسي</div><div class="info-value" id="termBox"></div></div>
  <div class="info-box"><div class="info-title">الصف</div><div class="info-value" id="gradeBox"></div></div>
  <div class="info-box"><div class="info-title">العدد</div><div class="info-value" id="countBox"></div></div>
  <!-- التعديل: المستهدفون الآن في الصف الأول بدلاً من نوع التقرير -->
  <div class="info-box"><div class="info-title">المستهدفون</div><div class="info-value" id="targetBox"></div></div>
</div>

<!-- تم تبديل نوع التقرير مكان المستهدفون هنا -->
<div class="info-grid2">
  <!-- التعديل: نوع التقرير الآن في الصف الثاني بدلاً من المستهدفون -->
  <div class="info-box"><div class="info-title">نوع التقرير</div><div class="info-value" id="reportTypeBox"></div></div>

  <div class="subject-lesson-box">
    <div class="subject-lesson-title">المادة | الدرس</div>
    <div class="subject-lesson">
      <div id="subjectBox"></div>
      <div class="subject-divider"></div>
      <div id="lessonBox"></div>
    </div>
  </div>

  <div class="info-box"><div class="info-title">مكان التنفيذ</div><div class="info-value" id="placeBox"></div></div>
</div>

<div class="box-objective">
  <div class="box-title">الهدف التربوي</div>
  <div class="box-content" id="goalBox"></div>
</div>

<div class="row">
  <div class="box"><div class="box-title">النبذة</div><div class="box-content" id="summaryBox"></div></div>
  <div class="box"><div class="box-title">إجراءات التنفيذ</div><div class="box-content" id="stepsBox"></div></div>
</div>

<div class="row">
  <div class="box"><div class="box-title">الاستراتيجيات</div><div class="box-content" id="strategiesBox"></div></div>
  <div class="box"><div class="box-title">نقاط القوة</div><div class="box-content" id="strengthsBox"></div></div>
</div>

<div class="row">
  <div class="box"><div class="box-title">نقاط التحسين</div><div class="box-content" id="improveBox"></div></div>
  <div class="box"><div class="box-title">التوصيات</div><div class="box-content" id="recommBox"></div></div>
</div>

<div class="tools-box">
  <div class="tools-title">الأدوات والوسائل التعليمية</div>
  <div class="tools-list" id="toolsListBox"></div>
</div>

<div class="images">
  <div class="image-box" id="imgBox1"></div>
  <div class="image-box" id="imgBox2"></div>
</div>

<div class="signatures">
  <div class="signature-box">
    <div class="signature-role" id="teacherTypeBox"></div>
    <div class="signature-name" id="teacherBox"></div>
    <div class="sign-line"></div>
  </div>
  <div class="signature-box">
    <div class="signature-role" id="principalTypeBox"></div>
    <div class="signature-name" id="principalBox"></div>
    <div class="sign-line"></div>
  </div>
</div>

<div class="footer-box">
  وزارة التعليم – المملكة العربية السعودية
</div>

</div>

<script>
// ==================== متغيرات إدارة المفاتيح ====================
let teacherClickCount = 0;
let aiFill2ClickCount = 0;
let geminiKeys = [
    { id: 1, key: '', active: false, lastUsed: 0, dailyCount: 0, maxDaily: 1500 },
    { id: 2, key: '', active: false, lastUsed: 0, dailyCount: 0, maxDaily: 1500 },
    { id: 3, key: '', active: false, lastUsed: 0, dailyCount: 0, maxDaily: 1500 },
    { id: 4, key: '', active: false, lastUsed: 0, dailyCount: 0, maxDaily: 1500 }
];

// كائن يحتوي على جميع التقارير مصنفة
const allReportsByCategory = {
  "التقارير التعليمية الصفية": [
    "تقرير أنشطة صفية",
    "تقرير توزيع وقت الحصة",
    "تقرير درس تم تنفيذه",
    "تقرير تعليم تعاوني بين الطلاب",
    "تقرير المشاركات بين الطلاب",
    "تقرير توزيع المنهج",
    "تقرير الفصول المقلوبة",
    "تقرير تنفيذ درس تطبيقي",
    "تقرير تفعيل الفصول الافتراضية",
    "تقرير التعليم المدمج",
    "تقرير التعليم عن بعد",
    "تقرير استخدام أنظمة إدارة التعلم",
    "تقرير إدارة الوقت في الصف",
    "تقرير تنظيم البيئة الصفية",
    "تقرير إدارة الموارد التعليمية",
    "تقرير إدارة السلوك الصفي",
    "تقرير الأنشطة التفاعلية",
    "تقرير العروض العملية",
    "تقرير التعلم التعاوني",
    "تقرير التعلم الذاتي الموجه",
    "تقرير الألعاب التعليمية الرقمية",
    "تقرير التعلم بالأقران",
    "تقرير استراتيجيات التدريس المستخدمة",
    "تقرير تنويع أساليب الشرح",
    "تقرير مراعاة الفروق الفردية",
    "تقرير تفعيل مهارات التفكير",
    "تقرير دمج مهارات القرن الحادي والعشرين",
    "تقرير توظيف الوسائل التعليمية",
    "تقرير التهيئة الذهنية للدرس",
    "تقرير ختام الدرس والتقويم الختامي",
    "تقرير ربط الدرس بالحياة"
  ],
  "التقارير العلاجية والدعم الفردي": [
    "تقرير خطة علاجية",
    "تقرير سجل الخطط العلاجية",
    "تقرير رعاية الطلاب المتأخرين دراسيًا",
    "تقرير دراسة حالة",
    "تقرير معرفة الميول والاتجاهات",
    "تقرير التحليل الاحتياجات التدريبية",
    "تقرير دعم الطلاب ذوي الإعاقة",
    "تقرير خطة دعم فردية",
    "تقرير متابعة التحسن الأكاديمي",
    "تقرير تشخيص صعوبات التعلم",
    "تقرير برامج التقوية",
    "تقرير الإرشاد الأكاديمي الفردي",
    "تقرير متابعة الخطط العلاجية",
    "تقرير دعم الموهبة منخفضة التحصيل"
  ],
  "التقارير التحفيزية والسلوكية": [
    "تقرير تحفيز الطلاب",
    "تقرير تعزيز السلوك الإيجابي",
    "تقرير نظام الحوافز والمكافآت",
    "تقرير برنامج الدعم النفسي",
    "تقرير تحسين نتائج العلوم في الاختبارات الوطنية (نافس)",
    "تقرير تحسين نتائج الرياضيات في الاختبارات الوطنية (نافس)",
    "تقرير تحسين نتائج اللغة العربية في الاختبارات الوطنية (نافس)",
    "تقرير الانضباط المدرسي",
    "تقرير معالجة السلوكيات السلبية",
    "تقرير تعزيز الدافعية للتعلم",
    "تقرير بناء الاتجاهات الإيجابية",
    "تقرير متابعة السلوك الفردي",
    "تقرير برامج تعديل السلوك",
    "تقرير تعزيز القيم والاتجاهات"
  ],
  "تقارير الأنشطة غير الصفية": [
    "تقرير نشاط إثرائي",
    "تقرير رعاية الموهوبين",
    "تقرير المبادرات والابتكار",
    "تقرير تفعيل المنصات التعليمية",
    "تقرير حصة النشاط",
    "تقرير تفعيل حصص النشاط",
    "تقرير تنفيذ إذاعة مدرسية",
    "تقرير الزيارات الميدانية",
    "تقرير مبادرة تطوعية",
    "تقرير الاحتفال باليوم الوطني",
    "تقرير المعلم الصغير",
    "تقرير الأندية الطلابية",
    "تقرير المسابقات التعليمية",
    "تقرير الأنشطة الثقافية",
    "تقرير الأنشطة العلمية",
    "تقرير الأنشطة الرياضية",
    "تقرير الأنشطة الفنية",
    "تقرير المعارض المدرسية",
    "تقرير الأيام العالمية",
    "تقرير البرامج الموسمية"
  ],
  "تقارير التواصل مع أولياء الأمور والمجتمع": [
    "تقرير التواصل مع ولي الأمر",
    "تقرير إشعار ولي الأمر عن مستوى ابنه",
    "تقرير سجل التواصل مع أولياء الأمور",
    "تقرير حضور اجتماع أولياء الأمور",
    "تقرير الشراكات المهنية",
    "تقرير مجتمعات التعلم",
    "تقرير المجتمعات المهنية",
    "تقرير اللقاءات التربوية",
    "تقرير المبادرات المجتمعية",
    "تقرير التواصل الإلكتروني مع أولياء الأمور",
    "تقرير الزيارات المنزلية",
    "تقرير استطلاع رضا أولياء الأمور",
    "تقرير التعاون مع الجهات الخارجية",
    "تقرير العمل التطوعي المجتمعي"
  ],
  "التقارير التخطيطية والتنظيمية": [
    "تقرير خطة أسبوعية",
    "تقرير تفعيل الخطة الأسبوعية",
    "تقرير تخطيط المشاريع التعليمية",
    "تقرير تخطيط الرحلات التعليمية",
    "تقرير إدارة الاجتماعات",
    "تقرير المناوبة والفسحة",
    "تقرير الإشراف اليومي",
    "تقرير إدارة الأزمات",
    "تقرير الخطة الفصلية",
    "تقرير الخطة السنوية",
    "تقرير تنظيم الجداول الدراسية",
    "تقرير تنظيم المهام الإدارية",
    "تقرير توزيع الأدوار",
    "تقرير إدارة الوقت المدرسي",
    "تقرير متابعة تنفيذ الخطط"
  ],
  "تقارير التقييم والمتابعة": [
    "تقرير كشف المتابعة",
    "تقرير تصنيف الطلاب",
    "تقرير تنفيذ اختبار تحسن",
    "تقرير سجل الدرجات الإلكتروني",
    "تقرير تحليل النتائج",
    "تقرير مقارنة السلاسل الزمنية",
    "تقرير قياس الأثر التعليمي",
    "تقرير مؤشرات الأداء التعليمي",
    "تقرير تقييم المخرجات التعليمية",
    "تقرير تقييم المشاريع الطلابية",
    "تقرير تقييم الأداء العملي",
    "تقرير تقييم المحافظ الإلكترونية",
    "تقرير التقييم الإلكتروني",
    "تقرير تحليل نتائج الاختبارات التشخيصية",
    "تقرير تحليل الاختبارات التحصيلية",
    "تقرير متابعة مستوى الإتقان",
    "تقرير فجوات التعلم",
    "تقرير تقدم الطلاب",
    "تقرير تحليل بنود الاختبار",
    "تقرير متابعة نواتج التعلم"
  ],
  "تقارير التدريب والتطوير المهني": [
    "تقرير حضور دورات وورش تدريبية",
    "تقرير الورش التدريبية التي قدمتها",
    "تقرير التدريب على الاختبارات المعيارية",
    "تقرير التدريب على المناهج الحديثة",
    "تقرير نقل أثر التدريب",
    "تقرير متابعة الدورات العالمية",
    "تقرير التطوير المهني المستمر",
    "تقرير المشاركة في المؤتمرات التعليمية",
    "تقرير حضور الندوات العلمية",
    "تقرير المشاركة في البحث التربوي",
    "تقرير التعلم الذاتي المهني",
    "تقرير مجتمعات التعلم المهنية",
    "تقرير القراءة التربوية المتخصصة",
    "تقرير تبادل الخبرات",
    "تقرير بناء المسار المهني"
  ],
  "تقارير توظيف التكنولوجيا": [
    "تقرير المحتوى الرقمي المنتج",
    "تقرير إنتاج المحتوى الرقمي",
    "تقرير استخدام أنظمة إدارة التعلم",
    "تقرير التقييم الإلكتروني",
    "تقرير الواقع المعزز في التعليم",
    "تقرير الألعاب التعليمية الرقمية",
    "تقرير توظيف الذكاء الاصطناعي",
    "تقرير التعلم المتنقل",
    "تقرير الصفوف الافتراضية",
    "تقرير أدوات التعلم التفاعلي",
    "تقرير الأمن الرقمي",
    "تقرير الثقافة الرقمية",
    "تقرير التحول الرقمي",
    "تقرير استخدام التطبيقات التعليمية"
  ],
  "تقارير البحث والتطوير المناهجي": [
    "تقرير تصميم الوحدات التعليمية",
    "تقرير إعداد المواد التعليمية",
    "تقرير تطوير المناهج الإثرائية",
    "تقرير إعداد بنك الأسئلة",
    "تقرير تصميم الأنشطة اللاصفية",
    "تقرير تحليل محتوى المنهج",
    "تقرير مواءمة المنهج مع نواتج التعلم",
    "تقرير تحديث الخطط الدراسية",
    "t report تطوير أدوات التقويم",
    "تقرير البحث الإجرائي"
  ],
  "تقارير الجودة واللجان": [
    "تقرير عضوية لجنة التميز والجودة",
    "تقرير عضوية لجنة التدقيق",
    "تقرير إدارة الموارد التعليمية",
    "تقرير تحسين الجودة",
    "تقرير متابعة مؤشرات الأداء",
    "تقرير التقييم الذاتي",
    "تقرير الاعتماد المدرسي",
    "تقرير الخطط التحسينية"
  ],
  "تقارير الأمن والسلامة": [
    "تقرير إجراءات السلامة في الصف",
    "تقرير الرعاية الصحية في المدرسة",
    "تقرير جرد المختبرات وغرف المصادر",
    "تقرير خطط الإخلاء",
    "تقرير السلامة المدرسية",
    "تقرير إدارة المخاطر",
    "تقرير الإسعافات الأولية",
    "تقرير جاهزية المباني"
  ]
}
// إنشاء قائمة بجميع التقارير لاستخدامها في البحث العام
const allReports = [];
for (const category in allReportsByCategory) {
    allReportsByCategory[category].forEach(report => {
        allReports.push({name: report, category: category});
    });
}

// كائن يحتوي على النصوص الذكية لكل نوع تقرير (تم إزالة المحتوى الخاص بتقرير نشاط إثرائي)
const autoTextsByReportType = {
    'تقرير نشاط إثرائي': {
        goal: [""],
        summary: [""],
        steps: [""],
        strategies: [""],
        strengths: [""],
        improve: [""],
        recomm: [""]
    }
};

// النصوص الافتراضية
const defaultTexts = {
    goal: ["الهدف التربوي"],
    summary: ["النبذة المختصرة"],
    steps: ["إجراءات التنفيذ"],
    strategies: ["الاستراتيجيات"],
    strengths: ["نقاط القوة"],
    improve: ["نقاط التحسين"],
    recomm: ["التوصيات"]
};

let counters = {goal:0,summary:0,steps:0,strategies:0,strengths:0,improve:0,recomm:0};
let currentReportType = "";

// متغيرات للتحكم بالتاريخ
let dateMode = 'hijri'; // hijri أو gregorian
let currentHijriDate = '';
let currentGregorianDate = '';

// رابط خادم الذكاء الاصطناعي B الصحيح
const backendAIUrl = 'https://gemini-backend-x1r2.onrender.com/ask';

// ==================== دوال إدارة المفاتيح ====================

// دالة فتح/إغلاق لوحة التحكم الإداري
function openAdminPanel() {
    document.getElementById('adminOverlay').classList.add('show');
    document.getElementById('adminPanel').classList.add('show');
    document.body.style.overflow = 'hidden';
    loadKeysFromStorage();
}

function closeAdminPanel() {
    document.getElementById('adminOverlay').classList.remove('show');
    document.getElementById('adminPanel').classList.remove('show');
    document.body.style.overflow = 'auto';
}

// دالة التعامل مع النقر على عنوان المعلم
function handleTeacherTitleClick() {
    teacherClickCount++;
    
    if (teacherClickCount === 3) {
        // إظهار زر التعبئة الذكية ٢
        document.getElementById('aiFillBtn2').classList.add('show');
        showNotification('تم تفعيل زر التعبئة الذكية ٢! ✓');
    } else if (teacherClickCount >= 5) {
        // إظهار لوحة التحكم الإداري
        openAdminPanel();
        teacherClickCount = 0; // إعادة تعيين العداد
    }
}

// تحميل المفاتيح من التخزين المحلي
function loadKeysFromStorage() {
    const savedKeys = localStorage.getItem('geminiKeys');
    if (savedKeys) {
        const parsedKeys = JSON.parse(savedKeys);
        geminiKeys = parsedKeys;
        
        // تحديث واجهة المستخدم
        geminiKeys.forEach(key => {
            if (key.key) {
                document.getElementById(`keyInput${key.id}`).value = key.key;
                updateKeyStatus(key.id, key.active ? 'active' : 'inactive');
                document.getElementById(`toggleKey${key.id}`).classList.toggle('active', key.active);
                document.getElementById(`toggleKey${key.id}`).innerHTML = 
                    key.active ? '<i class="fas fa-power-off"></i> مفعل' : '<i class="fas fa-power-off"></i> تفعيل';
            }
        });
        
        showNotification('تم تحميل المفاتيح المحفوظة ✓');
    }
}

// حفظ المفاتيح في التخزين المحلي
function saveKeys() {
    // تحديث قيم المفاتيح من الحقول
    geminiKeys.forEach(key => {
        key.key = document.getElementById(`keyInput${key.id}`).value.trim();
    });
    
    localStorage.setItem('geminiKeys', JSON.stringify(geminiKeys));
    showNotification('تم حفظ المفاتيح بنجاح ✓');
}

// التحقق من صحة المفتاح
async function checkKey(keyId) {
    const keyInput = document.getElementById(`keyInput${keyId}`);
    const keyValue = keyInput.value.trim();
    
    if (!keyValue) {
        updateKeyStatus(keyId, 'inactive', 'المفتاح فارغ');
        return;
    }
    
    try {
        // اختبار المفتاح بمحاولة استدعاء بسيطة
        const testResponse = await fetch('https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key=' + keyValue, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                contents: [{
                    parts: [{
                        text: "Hello"
                    }]
                }]
            })
        });
        
        if (testResponse.ok) {
            updateKeyStatus(keyId, 'active', 'المفتاح صالح');
            geminiKeys[keyId - 1].key = keyValue;
        } else {
            updateKeyStatus(keyId, 'inactive', 'المفتاح غير صالح');
        }
    } catch (error) {
        updateKeyStatus(keyId, 'inactive', 'خطأ في الاتصال');
    }
}

// تفعيل/إلغاء تفعيل المفتاح
function toggleKey(keyId) {
    const button = document.getElementById(`toggleKey${keyId}`);
    const keyIndex = keyId - 1;
    
    geminiKeys[keyIndex].active = !geminiKeys[keyIndex].active;
    
    if (geminiKeys[keyIndex].active) {
        button.classList.add('active');
        button.innerHTML = '<i class="fas fa-power-off"></i> مفعل';
        updateKeyStatus(keyId, 'active', 'المفتاح مفعل');
    } else {
        button.classList.remove('active');
        button.innerHTML = '<i class="fas fa-power-off"></i> تفعيل';
        updateKeyStatus(keyId, 'inactive', 'المفتاح معطل');
    }
}

// تحديث حالة المفتاح في الواجهة
function updateKeyStatus(keyId, status, message = '') {
    const statusElement = document.getElementById(`keyStatus${keyId}`);
    statusElement.className = `key-status ${status}`;
    statusElement.innerHTML = `<i class="fas fa-${status === 'active' ? 'check-circle' : 'exclamation-circle'}"></i> ${message}`;
}

// تفعيل جميع المفاتيح
function activateAllKeys() {
    geminiKeys.forEach((key, index) => {
        if (key.key) {
            key.active = true;
            const button = document.getElementById(`toggleKey${key.id}`);
            button.classList.add('active');
            button.innerHTML = '<i class="fas fa-power-off"></i> مفعل';
            updateKeyStatus(key.id, 'active', 'مفعل');
        }
    });
    showNotification('تم تفعيل جميع المفاتيح ✓');
}

// إلغاء تفعيل جميع المفاتيح
function deactivateAllKeys() {
    geminiKeys.forEach(key => {
        key.active = false;
        const button = document.getElementById(`toggleKey${key.id}`);
        button.classList.remove('active');
        button.innerHTML = '<i class="fas fa-power-off"></i> تفعيل';
        updateKeyStatus(key.id, 'inactive', 'معطل');
    });
    showNotification('تم إلغاء تفعيل جميع المفاتيح ✓');
}

// الحصول على المفتاح التالي المتاح
function getNextAvailableKey() {
    const now = Date.now();
    const oneDay = 24 * 60 * 60 * 1000;
    
    // تصفية المفاتيح النشطة والمرتبة حسب الاستخدام
    const availableKeys = geminiKeys.filter(key => 
        key.active && key.key && 
        (now - key.lastUsed > oneDay || key.dailyCount < key.maxDaily)
    );
    
    if (availableKeys.length === 0) {
        return null;
    }
    
    // ترتيب المفاتيح حسب عدد الاستخدامات اليومية (الأقل استخدامًا أولاً)
    availableKeys.sort((a, b) => a.dailyCount - b.dailyCount);
    
    return availableKeys[0];
}

// تحديث استخدام المفتاح
function updateKeyUsage(keyId) {
    const now = Date.now();
    const oneDay = 24 * 60 * 60 * 1000;
    const keyIndex = keyId - 1;
    
    // إعادة تعيين العداد اليومي إذا مر يوم
    if (now - geminiKeys[keyIndex].lastUsed > oneDay) {
        geminiKeys[keyIndex].dailyCount = 0;
    }
    
    geminiKeys[keyIndex].dailyCount++;
    geminiKeys[keyIndex].lastUsed = now;
    
    // حفظ التحديثات
    localStorage.setItem('geminiKeys', JSON.stringify(geminiKeys));
}

// ==================== دالة التعبئة الذكية ٢ (باستخدام المفاتيح المتعددة) ====================
async function fillWithAI2() {
    // التحقق من اتصال الإنترنت
    if (!navigator.onLine) {
        alert('لا يوجد اتصال بالإنترنت. الرجاء التأكد من الاتصال');
        return;
    }
    
    // الحصول على نوع التقرير
    const reportType = getReportTypeText();
    if (!reportType || reportType === 'تقرير') {
        alert('الرجاء اختيار أو إدخال نوع التقرير أولاً');
        return;
    }
    
    // التحقق من وجود مفاتيح مفعلة
    const availableKey = getNextAvailableKey();
    if (!availableKey) {
        alert('لا توجد مفاتيح جيمني مفعلة أو أن جميعها تجاوز الحد اليومي. الرجاء تفعيل مفاتيح جديدة من لوحة التحكم.');
        return;
    }
    
    // الحصول على معلومات إضافية
    const subject = document.getElementById('subject').value || '';
    const lesson = document.getElementById('lesson').value || '';
    const grade = document.getElementById('grade').value || '';
    const target = document.getElementById('target').value || '';
    const place = document.getElementById('place').value || '';
    const count = document.getElementById('count').value || '';
    
    // عرض مؤشر التحميل
    const aiButton = document.getElementById('aiFillBtn2');
    const originalText = aiButton.querySelector('.btn-text').textContent;
    const originalIcon = aiButton.querySelector('.btn-icon').className;
    
    // إضافة مؤشر التحميل
    let loadingIndicator = aiButton.querySelector('.ai-loading-indicator');
    if (!loadingIndicator) {
        loadingIndicator = document.createElement('div');
        loadingIndicator.className = 'ai-loading-indicator';
        aiButton.appendChild(loadingIndicator);
    }
    loadingIndicator.style.display = 'block';
    
    aiButton.querySelector('.btn-text').textContent = 'جارٍ التعبئة...';
    aiButton.querySelector('.btn-icon').className = 'fas fa-spinner fa-spin btn-icon';
    aiButton.classList.add('ai-loading');
    aiButton.disabled = true;
    
    try {
        // إعداد النص المطلوب للذكاء الاصطناعي - البرومبت المهني
        const prompt = `أنت خبير تربوي تعليمي محترف تمتلك خبرة ميدانية واسعة في التعليم العام.  
اعتمد منظورًا تربويًا مهنيًا احترافيًا يركّز على تحسين جودة التعليم، ودعم المعلم، وتعزيز بيئة التعلّم، وخدمة القيادة المدرسية.  

التقرير المطلوب: "${reportType}"
${subject ? `المادة: ${subject}` : ''}
${lesson ? `الدرس: ${lesson}` : ''}
${grade ? `الصف: ${grade}` : ''}
${target ? `المستهدفون: ${target}` : ''}
${place ? `مكان التنفيذ: ${place}` : ''}
${count ? `عدد الحضور: ${count}` : ''}

**توجيهات مهنية:**
- كن موضوعيًا ومتزنًا وبنّاءً  
- قدّم الملاحظات بصيغة تطويرية غير نقدية  
- راعِ واقع الميدان التعليمي وسياق المدرسة  
- اربط بين المعلم والطالب والمنهج والبيئة الصفية والقيادة المدرسية  
- ركّز على جودة التعليم وأثر الممارسات على تعلم الطلاب  
- التزم بلغة عربية فصيحة سليمة وخالية من الأخطاء  

**شروط المحتوى:**اكتب محتوى كل حقل بصيغة تقريرية مهنية وكأنه صادر عن المعلم.
لا تكتب أبداً عنوان الحقل داخل المحتوى ولا تعِد صياغته بصيغة مباشرة (مثل: الهدف التربوي هو، النبذة المختصرة).
يجب أن يحتوي كل حقل على ما يقارب 25 كلمة.
ابدأ بالمضمون مباشرة دون تمهيد أو عبارات إنشائية.
يمكن الاستفادة من معنى العنوان أو أحد مفاهيمه بشكل غير مباشر فقط عند الحاجة وبما يخدم الفكرة دون تكرار أو حشو.
احرص على وجود ترابط منطقي بين الأهداف، النبذة المختصرة، الاستراتيجيات، إجراءات التنفيذ، نقاط القوة، نقاط التحسين، والتوصيات.
اربط المحتوى بالمادة الدراسية وعنوان الدرس إن وُجد، وكذلك بمكان التنفيذ، بأسلوب مهني متوازن يجمع بين الإشارة المباشرة وغير المباشرة دون تكلف.
اجعل الهدف النهائي للمحتوى تحسين الممارسة التعليمية ودعم التطوير المهني المستدام.
راعِ الوضوح والترابط، واجعل كل جملة تضيف قيمة تعليمية فعلية.
الحقول المطلوبة:**
1. الهدف التربوي
2. نبذة مختصرة  
3. إجراءات التنفيذ
4. الاستراتيجيات
5. نقاط القوة
6. نقاط التحسين
7. التوصيات

يرجى تقديم الإجابة باللغة العربية الفصحى، وتنظيمها بحيث يكون كل حقل في سطر منفصل يبدأ برقمه فقط دون ذكر العنوان.`;

        // استخدام API جيمني مباشرة مع المفتاح المحدد
        const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-lite:generateContent?key=${availableKey.key}`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                contents: [{
                    parts: [{
                        text: prompt
                    }]
                }],
                generationConfig: {
                    temperature: 0.7,
                    topK: 40,
                    topP: 0.95,
                    maxOutputTokens: 2048,
                }
            })
        });

        if (!response.ok) {
            throw new Error(`خطأ في المفتاح ${availableKey.id}: ${response.status}`);
        }

        const data = await response.json();
        
        // فحص الاستجابة
        if (!data || !data.candidates || !data.candidates[0] || !data.candidates[0].content || !data.candidates[0].content.parts) {
            throw new Error('لم يتم الحصول على إجابة صحيحة من الذكاء الاصطناعي');
        }
        
        const aiResponse = data.candidates[0].content.parts[0].text;
        
        // تحديث استخدام المفتاح
        updateKeyUsage(availableKey.id);
        
        // تحليل الإجابة وتعيينها للحقول
        parseAIResponseProfessional(aiResponse);
        
        showNotification(`تم تعبئة الحقول باستخدام المفتاح ${availableKey.id} ✓ (${availableKey.dailyCount}/${availableKey.maxDaily})`);
        
    } catch (error) {
        console.error('خطأ في الذكاء الاصطناعي:', error);
        
        // محاولة استخدام مفتاح آخر إذا فشل الأول
        if (error.message.includes('خطأ في المفتاح')) {
            // تعطيل المفتاح الحالي
            geminiKeys[availableKey.id - 1].active = false;
            localStorage.setItem('geminiKeys', JSON.stringify(geminiKeys));
            
            // تحديث الواجهة
            const button = document.getElementById(`toggleKey${availableKey.id}`);
            if (button) {
                button.classList.remove('active');
                button.innerHTML = '<i class="fas fa-power-off"></i> تفعيل';
                updateKeyStatus(availableKey.id, 'inactive', 'تجاوز الحد اليومي');
            }
            
            // محاولة مرة أخرى بمفتاح آخر
            setTimeout(() => {
                fillWithAI2();
            }, 1000);
        } else {
            alert(`خطأ: ${error.message}\n\nتأكد من:\n1. اتصال الإنترنت\n2. صحة المفاتيح المدخلة`);
        }
    } finally {
        // إخفاء مؤشر التحميل
        if (loadingIndicator) {
            loadingIndicator.style.display = 'none';
        }
        
        // استعادة الحالة الأصلية
        aiButton.querySelector('.btn-text').textContent = originalText;
        aiButton.querySelector('.btn-icon').className = originalIcon;
        aiButton.classList.remove('ai-loading');
        aiButton.disabled = false;
    }
}

// ==================== دوال التحويل والتواريخ ====================
async function convertHijriToGregorian(hijriDate) {
    if (!hijriDate || hijriDate.trim() === '') return '';
    
    try {
        // تحويل الأرقام العربية إلى إنجليزية
        const arabicToEnglish = {
            '٠': '0', '١': '1', '٢': '2', '٣': '3', '٤': '4',
            '٥': '5', '٦': '6', '٧': '7', '٨': '8', '٩': '9'
        };
        
        let cleanDate = hijriDate;
        for (let arabic in arabicToEnglish) {
            cleanDate = cleanDate.replace(new RegExp(arabic, 'g'), arabicToEnglish[arabic]);
        }
        
        const dateParts = cleanDate.split(/[-\/]/);
        if (dateParts.length === 3) {
            const day = parseInt(dateParts[2]);
            const month = parseInt(dateParts[1]);
            const year = parseInt(dateParts[0]);
            
            if (!isNaN(day) && !isNaN(month) && !isNaN(year)) {
                const response = await fetch(`https://api.aladhan.com/v1/hToG?date=${day}-${month}-${year}`);
                if (response.ok) {
                    const data = await response.json();
                    if (data.data && data.data.gregorian) {
                        const g = data.data.gregorian;
                        return `${g.day}/${g.month.number}/${g.year}`;
                    }
                }
            }
        }
        
        return hijriDate;
        
    } catch (error) {
        console.error('خطأ في تحويل التاريخ:', error);
        return hijriDate;
    }
}

function getCurrentTexts() {
    const reportType = document.getElementById('reportType').value;
    return autoTextsByReportType[reportType] || defaultTexts;
}

function autoFill(id){
    const texts = getCurrentTexts();
    if (texts[id] && texts[id].length > 0) {
        counters[id] = (counters[id] + 1) % texts[id].length;
        document.getElementById(id).value = texts[id][counters[id]];
        updateReport();
    } else {
        alert("لا توجد نصوص ذكية متاحة لهذا الحقل في التقرير الحالي");
    }
}

// دالة معالجة اختيار التصنيف
function handleReportCategory() {
    const categorySelect = document.getElementById('reportCategory');
    const reportTypeSelect = document.getElementById('reportType');
    const reportTypeInput = document.getElementById('reportTypeInput');
    const reportSearchContainer = document.getElementById('reportSearchContainer');
    const manualTitleContainer = document.querySelector('.manual-title-container');
    
    if (categorySelect.value === "أخرى") {
        reportTypeSelect.style.display = 'none';
        reportTypeInput.style.display = 'block';
        reportSearchContainer.style.display = 'none';
        manualTitleContainer.style.display = 'block';
        reportTypeSelect.innerHTML = '<option value="أخرى">أخرى</option>';
        reportTypeSelect.value = "أخرى";
        handleReportType();
    } else if (categorySelect.value) {
        reportTypeSelect.style.display = 'block';
        reportTypeInput.style.display = 'none';
        reportSearchContainer.style.display = 'block';
        manualTitleContainer.style.display = 'block';
        const reports = allReportsByCategory[categorySelect.value] || [];
        updateReportTypeOptions(reports);
        document.getElementById('reportSearch').value = '';
        document.getElementById('searchResults').style.display = 'none';
    } else {
        reportTypeSelect.style.display = 'none';
        reportTypeInput.style.display = 'none';
        reportSearchContainer.style.display = 'none';
        manualTitleContainer.style.display = 'block';
        reportTypeSelect.innerHTML = '<option value="">اختر تقريرًا</option>';
    }
}

// دالة تحديث خيارات قائمة التقارير
function updateReportTypeOptions(reports) {
    const reportTypeSelect = document.getElementById('reportType');
    reportTypeSelect.innerHTML = '<option value="">اختر تقريرًا</option>';
    
    reports.forEach(report => {
        const option = document.createElement('option');
        option.value = report;
        option.textContent = report;
        reportTypeSelect.appendChild(option);
    });
}

// دالة البحث الفوري في التقارير
function handleReportSearch() {
    const reportSearch = document.getElementById('reportSearch');
    const searchResults = document.getElementById('searchResults');
    const categorySelect = document.getElementById('reportCategory');
    const reportTypeSelect = document.getElementById('reportType');
    
    const searchTerm = reportSearch.value.trim().toLowerCase();
    
    if (searchTerm === '') {
        searchResults.style.display = 'none';
        searchResults.innerHTML = '';
        return;
    }
    
    let filteredReports = [];
    
    if (categorySelect.value && categorySelect.value !== "أخرى") {
        const reports = allReportsByCategory[categorySelect.value] || [];
        filteredReports = reports.filter(report => 
            report.toLowerCase().includes(searchTerm)
        );
    } else if (categorySelect.value === "أخرى") {
        filteredReports = [];
    } else {
        filteredReports = allReports.filter(item => 
            item.name.toLowerCase().includes(searchTerm)
        );
    }
    
    if (filteredReports.length > 0) {
        searchResults.innerHTML = '';
        
        filteredReports.forEach(item => {
            const reportName = typeof item === 'string' ? item : item.name;
            const reportCategory = typeof item === 'string' ? categorySelect.value : item.category;
            
            const div = document.createElement('div');
            div.textContent = reportName;
            div.style.padding = '8px 12px';
            div.style.cursor = 'pointer';
            div.style.borderBottom = '1px solid #eee';
            div.setAttribute('data-category', reportCategory);
            div.setAttribute('data-report', reportName);
            
            div.onmouseover = () => div.style.backgroundColor = '#f0f9f6';
            div.onmouseout = () => div.style.backgroundColor = 'white';
            div.onclick = () => {
                const selectedReport = div.getAttribute('data-report');
                const selectedCategory = div.getAttribute('data-category');
                
                if (categorySelect.value !== selectedCategory && selectedCategory) {
                    categorySelect.value = selectedCategory;
                    const reports = allReportsByCategory[selectedCategory] || [];
                    updateReportTypeOptions(reports);
                }
                
                reportTypeSelect.value = selectedReport;
                reportSearch.value = '';
                searchResults.style.display = 'none';
                handleReportType();
                updateReport();
                reportTypeSelect.style.display = 'block';
                reportTypeSelect.style.borderColor = '#066d4d';
                setTimeout(() => {
                    reportTypeSelect.style.borderColor = '#d4ebe2';
                }, 1000);
            };
            searchResults.appendChild(div);
        });
        searchResults.style.display = 'block';
    } else {
        searchResults.innerHTML = '<div style="padding:12px; color:#666; text-align:center;">لا توجد نتائج</div>';
        searchResults.style.display = 'block';
    }
}

// دالة لتحديث عنوان التقرير اليدوي
function updateManualTitle() {
    updateReport();
}

// دالة تكيف الخطوط
function adaptSubjectLessonFont() {
  const elements = [
    document.getElementById('subjectBox'),
    document.getElementById('lessonBox')
  ];

  elements.forEach(el => {
    if (!el || !el.parentElement) return;

    const text = el.innerText.trim();
    const textLength = text.length;
    const container = el.parentElement;
    const containerWidth = container.clientWidth - 30;
    const containerHeight = container.clientHeight - 20;

    el.style.whiteSpace = 'nowrap';
    el.style.overflow = 'hidden';
    el.style.textOverflow = 'ellipsis';
    el.style.textAlign = 'center';
    el.style.display = 'flex';
    el.style.alignItems = 'center';
    el.style.justifyContent = 'center';
    el.style.padding = '0 5px';
    el.style.width = '100%';
    el.style.height = '100%';

    if (!text || text === 'غير محدد' || textLength === 0) {
      el.style.fontSize = '11px';
      el.style.fontWeight = '600';
      el.style.lineHeight = '1.2';
      return;
    }

    const approxCharWidth = 7;
    const approxTextWidth = textLength * approxCharWidth;
    const widthRatio = approxTextWidth / containerWidth;
    
    let fontSize, fontWeight, lineHeight;
    
    if (widthRatio > 2.0) {
      fontSize = '7px';
      fontWeight = '600';
      lineHeight = '1.0';
      el.style.whiteSpace = 'normal';
      el.style.overflow = 'hidden';
      el.style.display = '-webkit-box';
      el.style.WebkitLineClamp = '2';
      el.style.WebkitBoxOrient = 'vertical';
    } else if (widthRatio > 1.2) {
      fontSize = '8px';
      fontWeight = '600';
      lineHeight = '1.1';
    } else if (widthRatio > 0.8) {
      fontSize = '9px';
      fontWeight = '700';
      lineHeight = '1.2';
    } else if (widthRatio > 0.5) {
      fontSize = '10px';
      fontWeight = '800';
      lineHeight = '1.3';
    } else {
      fontSize = '12px';
      fontWeight = '900';
      lineHeight = '1.4';
    }

    if (containerHeight < 30) {
      fontSize = Math.min(parseInt(fontSize), 9) + 'px';
      lineHeight = '1.1';
    }

    el.style.fontSize = fontSize;
    el.style.fontWeight = fontWeight;
    el.style.lineHeight = lineHeight;
  });
}

function adaptSubjectLessonFontWithRetry() {
  adaptSubjectLessonFont();
  setTimeout(adaptSubjectLessonFont, 100);
  
  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', adaptSubjectLessonFont);
  }
}

// دالة تحديث التاريخ اليدوي
async function updateManualDate() {
    const manualDate = document.getElementById('manualDateInput').value;
    if (manualDate && manualDate.trim() !== '') {
        const hijriDate = manualDate.trim();
        currentHijriDate = hijriDate;
        
        try {
            const gregorianDate = await convertHijriToGregorian(hijriDate);
            currentGregorianDate = gregorianDate;
            
            document.getElementById('currentDateDisplay').textContent = `هـ: ${hijriDate}`;
            document.getElementById('hDate').innerHTML = hijriDate + " هـ";
            document.getElementById('gDate').innerHTML = gregorianDate + " م";
            
            showNotification('تم تحديث التاريخين (الهجري والميلادي) ✓');
        } catch (error) {
            document.getElementById('currentDateDisplay').textContent = `هجري: ${hijriDate}`;
            document.getElementById('hDate').innerHTML = hijriDate + " هـ";
            document.getElementById('gDate').innerHTML = hijriDate + " هـ";
            showNotification('تم تحديث التاريخ الهجري ✓ (تعذر تحويل التاريخ الميلادي)');
        }
        
        const savedData = localStorage.getItem('teacherData');
        if (savedData) {
            const teacherData = JSON.parse(savedData);
            teacherData.manualHijriDate = currentHijriDate;
            teacherData.manualGregorianDate = currentGregorianDate;
            localStorage.setItem('teacherData', JSON.stringify(teacherData));
        }
    }
}

// دالة تبديل حالة الأداة
function toggleTool(toolElement) {
    const checkbox = toolElement.querySelector('input[type="checkbox"]');
    checkbox.checked = !checkbox.checked;
    
    if (checkbox.checked) {
        toolElement.classList.add('checked');
    } else {
        toolElement.classList.remove('checked');
    }
    
    updateToolsDisplay();
}

function updateReport(){
    document.getElementById('educationBox').innerText = document.getElementById('education').value;
    document.getElementById('schoolBox').innerText = document.getElementById('school').value;
    
    const termValue = document.getElementById('term').value;
    document.getElementById('termBox').innerText = termValue ? `الفصل الدراسي ${termValue}` : 'غير محدد';
    document.getElementById('gradeBox').innerText = document.getElementById('grade').value || 'غير محدد';
    document.getElementById('countBox').innerText = document.getElementById('count').value || 'غير محدد';
    document.getElementById('reportTypeBox').innerText = getReportTypeText();
    document.getElementById('targetBox').innerText = document.getElementById('target').value || 'غير محدد';
    document.getElementById('placeBox').innerText = document.getElementById('place').value || 'غير محدد';
    document.getElementById('subjectBox').innerText = document.getElementById('subject').value || 'غير محدد';
    document.getElementById('lessonBox').innerText = document.getElementById('lesson').value || 'غير محدد';
    
    document.getElementById('teacherBox').innerText = document.getElementById('teacher').value;
    document.getElementById('principalBox').innerText = document.getElementById('principal').value;
    document.getElementById('teacherTypeBox').innerText = document.getElementById('teacherType').value;
    document.getElementById('principalTypeBox').innerText = document.getElementById('principalType').value;
    
    document.getElementById('goalBox').innerText = document.getElementById('goal').value || 'لم يتم تحديد الهدف التربوي';
    document.getElementById('summaryBox').innerText = document.getElementById('summary').value || 'لم يتم إضافة نبذة مختصرة';
    document.getElementById('stepsBox').innerText = document.getElementById('steps').value || 'لم يتم تحديد إجراءات التنفيذ';
    document.getElementById('strategiesBox').innerText = document.getElementById('strategies').value || 'لم يتم تحديد الاستراتيجيات';
    document.getElementById('strengthsBox').innerText = document.getElementById('strengths').value || 'لم يتم تحديد نقاط القوة';
    document.getElementById('improveBox').innerText = document.getElementById('improve').value || 'لم يتم تحديد نقاط التحسين';
    document.getElementById('recommBox').innerText = document.getElementById('recomm').value || 'لم يتم تحديد التوصيات';
    
    updateToolsDisplay();
    setTimeout(adaptSubjectLessonFontWithRetry, 10);
}

function getReportTypeText() {
    const reportTypeSelect = document.getElementById('reportType');
    const reportTypeInput = document.getElementById('reportTypeInput');
    const manualTitleInput = document.getElementById('manualReportTitle');
    const categorySelect = document.getElementById('reportCategory');
    
    if (manualTitleInput && manualTitleInput.value.trim()) {
        return manualTitleInput.value.trim();
    }
    
    if (categorySelect.value === "أخرى") {
        return reportTypeInput.value || "تقرير";
    } else {
        return reportTypeSelect.value || "تقرير";
    }
}

function updateToolsDisplay() {
    const toolsListBox = document.getElementById('toolsListBox');
    toolsListBox.innerHTML = '';
    
    const selectedTools = [];
    
    for (let i = 1; i <= 10; i++) {
        const toolCheckbox = document.getElementById(`tool${i}`);
        if (toolCheckbox && toolCheckbox.checked) {
            selectedTools.push(toolCheckbox.value);
        }
    }
    
    selectedTools.forEach(tool => {
        const toolElement = document.createElement('div');
        toolElement.className = 'tool';
        toolElement.innerHTML = `<span>✓</span> ${tool}`;
        toolsListBox.appendChild(toolElement);
    });
    
    if (selectedTools.length === 0) {
        const noToolsMessage = document.createElement('div');
        noToolsMessage.style.textAlign = 'center';
        noToolsMessage.style.color = '#666';
        noToolsMessage.style.fontSize = '9px';
        noToolsMessage.style.padding = '4px';
        noToolsMessage.textContent = 'لم يتم اختيار أي أدوات';
        toolsListBox.appendChild(noToolsMessage);
    }
}

function loadImage(input, target) {
    if (input.files && input.files[0]) {
        const reader = new FileReader();
        reader.onload = function(e) {
            const imgBox = document.getElementById(target);
            imgBox.innerHTML = '';

            const img = document.createElement('img');
            img.src = e.target.result;
            img.loading = "eager";
            img.decoding = "sync";

            imgBox.appendChild(img);
        };
        reader.readAsDataURL(input.files[0]);
    }
}

// دالة جديدة لحفظ بيانات المعلم فقط
function saveTeacherData(){
    const teacherData = {
        education: document.getElementById('education').value,
        school: document.getElementById('school').value,
        grade: document.getElementById('grade').value,
        subject: document.getElementById('subject').value,
        target: document.getElementById('target').value,
        place: document.getElementById('place').value,
        lesson: document.getElementById('lesson').value,
        teacher: document.getElementById('teacher').value,
        principal: document.getElementById('principal').value,
        teacherType: document.getElementById('teacherType').value,
        principalType: document.getElementById('principalType').value,
        term: document.getElementById('term').value,
        count: document.getElementById('count').value,
        manualTitle: document.getElementById('manualReportTitle').value,
        manualHijriDate: currentHijriDate,
        manualGregorianDate: currentGregorianDate,
        tools: []
    };
    
    for (let i = 1; i <= 10; i++) {
        const toolCheckbox = document.getElementById(`tool${i}`);
        if (toolCheckbox && toolCheckbox.checked) {
            teacherData.tools.push(toolCheckbox.value);
        }
    }
    
    const textFields = ['goal', 'summary', 'steps', 'strategies', 'strengths', 'improve', 'recomm'];
    textFields.forEach(field => {
        teacherData[field] = document.getElementById(field).value;
    });
    
    localStorage.setItem('teacherData', JSON.stringify(teacherData));
    showNotification('تم حفظ بيانات المعلم بنجاح!');
}

// دالة لعرض الإشعارات
function showNotification(message) {
    const notification = document.getElementById('saveNotification');
    notification.querySelector('span').textContent = message;
    notification.classList.add('show');
    
    setTimeout(() => {
        notification.classList.remove('show');
    }, 3000);
}

// دالة لتحميل بيانات المعلم المحفوظة عند تشغيل الصفحة
function loadTeacherData() {
    const savedData = localStorage.getItem('teacherData');
    
    if (savedData) {
        const teacherData = JSON.parse(savedData);
        
        document.getElementById('education').value = teacherData.education || '';
        document.getElementById('school').value = teacherData.school || '';
        document.getElementById('grade').value = teacherData.grade || '';
        document.getElementById('subject').value = teacherData.subject || '';
        document.getElementById('target').value = teacherData.target || '';
        document.getElementById('place').value = teacherData.place || '';
        document.getElementById('lesson').value = teacherData.lesson || '';
        document.getElementById('teacher').value = teacherData.teacher || '';
        document.getElementById('principal').value = teacherData.principal || '';
        document.getElementById('teacherType').value = teacherData.teacherType || 'المعلم';
        document.getElementById('principalType').value = teacherData.principalType || 'المدير';
        document.getElementById('term').value = teacherData.term || '';
        document.getElementById('count').value = teacherData.count || '';
        document.getElementById('manualReportTitle').value = teacherData.manualTitle || '';
        
        if (teacherData.manualHijriDate) {
            currentHijriDate = teacherData.manualHijriDate;
            currentGregorianDate = teacherData.manualGregorianDate || '';
            document.getElementById('manualDateInput').value = currentHijriDate;
            document.getElementById('currentDateDisplay').textContent = `هجري: ${currentHijriDate}`;
            
            document.getElementById('hDate').innerHTML = currentHijriDate + " هـ";
            document.getElementById('gDate').innerHTML = currentGregorianDate ? currentGregorianDate + " م" : currentHijriDate + " هـ";
        }
        
        const textFields = ['goal', 'summary', 'steps', 'strategies', 'strengths', 'improve', 'recomm'];
        textFields.forEach(field => {
            if (teacherData[field]) {
                document.getElementById(field).value = teacherData[field];
            }
        });
        
        if (teacherData.tools && Array.isArray(teacherData.tools)) {
            for (let i = 1; i <= 10; i++) {
                const toolCheckbox = document.getElementById(`tool${i}`);
                if (toolCheckbox) {
                    const toolElement = toolCheckbox.closest('.tool-checkbox');
                    const isChecked = teacherData.tools.includes(toolCheckbox.value);
                    toolCheckbox.checked = isChecked;
                    if (isChecked) {
                        toolElement.classList.add('checked');
                    } else {
                        toolElement.classList.remove('checked');
                    }
                }
            }
        }
        
        updateReport();
        updateToolsDisplay();
    }
}

// دالة الذكاء الاصطناعي الأصلية
async function fillWithAI() {
    // التحقق من اتصال الإنترنت
    if (!navigator.onLine) {
        alert('لا يوجد اتصال بالإنترنت. الرجاء التأكد من الاتصال');
        return;
    }
    
    // الحصول على نوع التقرير
    const reportType = getReportTypeText();
    if (!reportType || reportType === 'تقرير') {
        alert('الرجاء اختيار أو إدخال نوع التقرير أولاً');
        return;
    }
    
    // الحصول على معلومات إضافية
    const subject = document.getElementById('subject').value || '';
    const lesson = document.getElementById('lesson').value || '';
    const grade = document.getElementById('grade').value || '';
    const target = document.getElementById('target').value || '';
    const place = document.getElementById('place').value || '';
    const count = document.getElementById('count').value || '';
    
    // عرض مؤشر التحميل
    const aiButton = document.getElementById('aiFillBtn');
    const originalText = aiButton.querySelector('.btn-text').textContent;
    const originalIcon = aiButton.querySelector('.btn-icon').className;
    
    let loadingIndicator = aiButton.querySelector('.ai-loading-indicator');
    if (!loadingIndicator) {
        loadingIndicator = document.createElement('div');
        loadingIndicator.className = 'ai-loading-indicator';
        aiButton.appendChild(loadingIndicator);
    }
    loadingIndicator.style.display = 'block';
    
    aiButton.querySelector('.btn-text').textContent = 'جارٍ التعبئة...';
    aiButton.querySelector('.btn-icon').className = 'fas fa-spinner fa-spin btn-icon';
    aiButton.classList.add('ai-loading');
    aiButton.disabled = true;
    
    try {
        const prompt = `أنت خبير تربوي تعليمي محترف تمتلك خبرة ميدانية واسعة في التعليم العام.  
اعتمد منظورًا تربويًا مهنيًا احترافيًا يركّز على تحسين جودة التعليم، ودعم المعلم، وتعزيز بيئة التعلّم، وخدمة القيادة المدرسية.  

التقرير المطلوب: "${reportType}"
${subject ? `المادة: ${subject}` : ''}
${lesson ? `الدرس: ${lesson}` : ''}
${grade ? `الصف: ${grade}` : ''}
${target ? `المستهدفون: ${target}` : ''}
${place ? `مكان التنفيذ: ${place}` : ''}
${count ? `عدد الحضور: ${count}` : ''}

**توجيهات مهنية:**
- كن موضوعيًا ومتزنًا وبنّاءً  
- قدّم الملاحظات بصيغة تطويرية غير نقدية  
- راعِ واقع الميدان التعليمي وسياق المدرسة  
- اربط بين المعلم والطالب والمنهج والبيئة الصفية والقيادة المدرسية  
- ركّز على جودة التعليم وأثر الممارسات على تعلم الطلاب  
- التزم بلغة عربية فصيحة سليمة وخالية من الأخطاء  

**شروط المحتوى:**اكتب محتوى كل حقل بصيغة تقريرية مهنية وكأنه صادر عن المعلم.
لا تكتب أبداً عنوان الحقل داخل المحتوى ولا تعِد صياغته بصيغة مباشرة (مثل: الهدف التربوي هو، النبذة المختصرة).
يجب أن يحتوي كل حقل على ما يقارب 25 كلمة.
ابدأ بالمضمون مباشرة دون تمهيد أو عبارات إنشائية.
يمكن الاستفادة من معنى العنوان أو أحد مفاهيمه بشكل غير مباشر فقط عند الحاجة وبما يخدم الفكرة دون تكرار أو حشو.
احرص على وجود ترابط منطقي بين الأهداف، النبذة المختصرة، الاستراتيجيات، إجراءات التنفيذ، نقاط القوة، نقاط التحسين، والتوصيات.
اربط المحتوى بالمادة الدراسية وعنوان الدرس إن وُجد، وكذلك بمكان التنفيذ، بأسلوب مهني متوازن يجمع بين الإشارة المباشرة وغير المباشرة دون تكلف.
اجعل الهدف النهائي للمحتوى تحسين الممارسة التعليمية ودعم التطوير المهني المستدام.
راعِ الوضوح والترابط، واجعل كل جملة تضيف قيمة تعليمية فعلية.
الحقول المطلوبة:**
1. الهدف التربوي
2. نبذة مختصرة  
3. إجراءات التنفيذ
4. الاستراتيجيات
5. نقاط القوة
6. نقاط التحسين
7. التوصيات

يرجى تقديم الإجابة باللغة العربية الفصحى، وتنظيمها بحيث يكون كل حقل في سطر منفصل يبدأ برقمه فقط دون ذكر العنوان.`;

        const response = await fetch(backendAIUrl, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                prompt: prompt,
                model: "gemini-2.5-flash-lite",
                reportData: {
                    reportType: reportType,
                    subject: subject,
                    lesson: lesson,
                    grade: grade,
                    target: target,
                    place: place,
                    count: count
                }
            })
        });

        if (!response.ok) {
            throw new Error(`خطأ في الاتصال بالخادم: ${response.status}`);
        }

        const data = await response.json();
        
        if (!data || !data.answer) {
            throw new Error('لم يتم الحصول على إجابة من الذكاء الاصطناعي');
        }
        
        const aiResponse = data.answer;
        parseAIResponseProfessional(aiResponse);
        showNotification('تم تعبئة الحقول باستخدام الذكاء الاصطناعي بنجاح! ✓');
        
    } catch (error) {
        console.error('خطأ في الذكاء الاصطناعي:', error);
        alert(`خطأ: ${error.message}\n\nتأكد من:\n1. اتصال الإنترنت\n2. أن خادم Backend يعمل على الرابط: ${backendAIUrl}`);
    } finally {
        if (loadingIndicator) {
            loadingIndicator.style.display = 'none';
        }
        
        aiButton.querySelector('.btn-text').textContent = originalText;
        aiButton.querySelector('.btn-icon').className = originalIcon;
        aiButton.classList.remove('ai-loading');
        aiButton.disabled = false;
    }
}

// دالة محسنة لتحليل استجابة الذكاء الاصطناعي المهنية
function parseAIResponseProfessional(response) {
    const lines = response.split('\n').filter(line => line.trim());
    
    const fieldMapping = {
        '1': 'goal',
        '2': 'summary', 
        '3': 'steps',
        '4': 'strategies',
        '5': 'strengths',
        '6': 'improve',
        '7': 'recomm'
    };
    
    let foundFields = 0;
    
    lines.forEach(line => {
        const match = line.match(/^(\d+)[\.\-]\s*(.+)/);
        if (match) {
            const fieldNumber = match[1];
            let content = match[2].trim();
            
            content = removeFieldTitles(content);
            
            if (fieldMapping[fieldNumber]) {
                const fieldId = fieldMapping[fieldNumber];
                content = ensureWordCount(content, 25);
                content = addProfessionalTouch(content, fieldId);
                
                document.getElementById(fieldId).value = content;
                foundFields++;
            }
        }
    });
    
    if (foundFields < 3) {
        fallbackProfessionalAIParsing(response);
    }
    
    updateReport();
}

// دالة لإزالة عناوين الحقول من النص
function removeFieldTitles(content) {
    const fieldTitles = [
        'الهدف التربوي', 'الهدف التربوي', ,
        'نبذة مختصرة', 'نبذة مختصرة', ,
        'إجراءات التنفيذ', 'إجراءات التنفيذ', ,
        'الاستراتيجيات', 'الاستراتيجيات',
        'نقاط القوة', 'نقاط القوة',
        'نقاط التحسين', 'نقاط تحسين',
        'التوصيات', 'التوصيات',
        'هو:', 'تشمل:', 'تشمل', 'يتضمن:', 'يتضمن',
        'يتمثل في', 'يتمثل', 'يمثل', 'يتم',
        'يشمل', 'تحتوي', 'تتضمن'
    ];
    
    let cleanedContent = content;
    
    fieldTitles.forEach(title => {
        const regex = new RegExp(`^${title}[:\\.\\-]?\\s*`, 'i');
        cleanedContent = cleanedContent.replace(regex, '');
        
        const regex2 = new RegExp(`\\s*${title}[:\\.\\-]?\\s*`, 'gi');
        cleanedContent = cleanedContent.replace(regex2, ' ');
    });
    
    cleanedContent = cleanedContent.trim().replace(/\s+/g, ' ');
    
    return cleanedContent || content;
}

// دالة لتأكيد عدد الكلمات مع لمسة مهنية
function ensureWordCount(content, targetWords) {
    const words = content.split(' ');
    
    if (words.length >= targetWords - 5 && words.length <= targetWords + 5) {
        return content;
    }
    
    if (words.length < targetWords - 5) {
        const professionalPhrases = [
            'مع التركيز على تحقيق أهداف التعلم وتنمية المهارات الأساسية',
            'بما يسهم في رفع مستوى التحصيل الدراسي وتحسين المخرجات التعليمية',
            'وذلك لتحقيق التكامل بين الجوانب المعرفية والمهارية والوجدانية',
            'مع مراعاة الفروق الفردية وتنويع أساليب التدريس لتناسب جميع الطلاب',
            'لضمان تحقيق رؤية التعليم وتطوير العملية التعليمية بصورة شاملة',
            'مع الاستفادة من أفضل الممارسات التربوية والتقنيات التعليمية الحديثة',
            'بما يعزز من دور المعلم كميسر للتعلم وموجه للطالب نحو التميز'
        ];
        
        let extendedContent = content;
        while (extendedContent.split(' ').length < targetWords) {
            const randomPhrase = professionalPhrases[Math.floor(Math.random() * professionalPhrases.length)];
            extendedContent += ' ' + randomPhrase;
        }
        
        const extendedWords = extendedContent.split(' ');
        if (extendedWords.length > targetWords + 5) {
            return extendedWords.slice(0, targetWords).join(' ');
        }
        
        return extendedContent;
    }
    
    if (words.length > targetWords + 5) {
        return words.slice(0, targetWords).join(' ');
    }
    
    return content;
}

// دالة لإضافة لمسة مهنية للمحتوى
function addProfessionalTouch(content, fieldId) {
    const words = content.split(' ');
    if (words.length >= 20) return content;
    
    const professionalAdditions = {
        'goal': ' بما يعزز من جودة التعليم ويدعم تحقيق رؤية المدرسة التعليمية',
        'summary': ' مع التركيز على الأثر الإيجابي في تحسين الممارسات التعليمية',
        'steps': ' ومراعاة الجوانب التربوية والنفسية للطلاب في جميع المراحل',
        'strategies': ' بما يناسب البيئة الصفية ويحقق أقصى استفادة تعليمية',
        'strengths': ' مما يسهم في تحقيق بيئة تعلم إيجابية ومنتجة',
        'improve': ' مع وضع خطط تطويرية قابلة للتنفيذ في الفصول القادمة',
        'recomm': ' بما يدعم التطوير المهني المستمر ويعزز جودة التعليم'
    };
    
    if (professionalAdditions[fieldId]) {
        return content + professionalAdditions[fieldId];
    }
    
    return content;
}

// نهج بديل محسن لتحليل الاستجابة المهنية
function fallbackProfessionalAIParsing(response) {
    const sentences = response.split(/[\.\n]/).filter(s => {
        const trimmed = s.trim();
        return trimmed.length > 20 && 
               !trimmed.match(/الهدف التربوي|نبذة مختصرة|إجراءات التنفيذ|الاستراتيجيات|نقاط القوة|نقاط التحسين|التوصيات|الحقل|المطلوب|يجب|يرجى/i);
    });
    
    const fields = ['goal', 'summary', 'steps', 'strategies', 'strengths', 'improve', 'recomm'];
    
    let sentenceIndex = 0;
    fields.forEach((field, index) => {
        if (sentenceIndex < sentences.length) {
            let content = sentences[sentenceIndex].trim();
            content = removeFieldTitles(content);
            content = ensureWordCount(content, 25);
            content = addProfessionalTouch(content, field);
            
            document.getElementById(field).value = content;
            sentenceIndex++;
        } else if (sentenceIndex > 0) {
            const previousContent = document.getElementById(fields[sentenceIndex-1]).value;
            if (previousContent) {
                const words = previousContent.split(' ');
                const modifiedContent = words.slice(5).join(' ') + ' مع التركيز على تطوير الممارسات التعليمية وتحسين جودة التعلم';
                document.getElementById(field).value = ensureWordCount(modifiedContent, 25);
            }
        }
    });
}

// وظائف الدعم الفني
function openSupportModal() {
    document.getElementById('supportModal').style.display = 'flex';
    document.body.style.overflow = 'hidden';
}

function closeSupportModal() {
    document.getElementById('supportModal').style.display = 'none';
    document.body.style.overflow = 'auto';
}

document.getElementById('supportModal').addEventListener('click', function(e) {
    if (e.target === this) {
        closeSupportModal();
    }
});

function sendEmailSupport() {
    const name = document.getElementById('supportName').value || 'مستخدم بدون اسم';
    const phone = document.getElementById('supportPhone').value || 'لم يتم تقديمه';
    const issue = document.getElementById('supportIssue').value || 'لا توجد تفاصيل';
    
    const subject = encodeURIComponent('طلب دعم فني - أداة إصدار التقارير');
    const body = encodeURIComponent(`الاسم: ${name}\nرقم التواصل: ${phone}\n\nتفاصيل المشكلة:\n${issue}\n\n---\nتم الإرسال من أداة إصدار التقارير`);
    
    window.location.href = `mailto:iFahadenglish@gmail.com?subject=${subject}&body=${body}`;
    setTimeout(closeSupportModal, 500);
}

function sendWhatsAppSupport() {
    const name = document.getElementById('supportName').value || 'مستخدم بدون اسم';
    const phone = document.getElementById('supportPhone').value || 'لم يتم تقديمه';
    const issue = document.getElementById('supportIssue').value || 'لا توجد تفاصيل';
    
    const message = encodeURIComponent(`طلب دعم فني - أداة إصدار التقارير\n\nالاسم: ${name}\nرقم التواصل: ${phone}\n\nتفاصيل المشكلة:\n${issue}\n\n---\nتم الإرسال من أداة إصدار التقارير`);
    
    window.open(`https://wa.me/966597077245?text=${message}`, '_blank');
    setTimeout(closeSupportModal, 500);
}

function clearData(){
    if(confirm("هل أنت متأكد من مسح جميع البيانات؟")){
        localStorage.clear();
        location.reload();
    }
}

async function downloadPDF(){
    document.querySelector('.control-bar').style.visibility = 'hidden';
    document.querySelector('.top-marquee').style.visibility = 'hidden';
    document.body.style.margin = "0";
    document.body.style.background = "white";

    const reportContent = document.getElementById('report-content');
    reportContent.style.display = 'block';
    reportContent.style.visibility = 'visible';
    reportContent.style.opacity = '1';
    reportContent.style.position = 'relative';
    reportContent.style.top = '0';
    reportContent.style.left = '0';

    const cleanFileName = getReportTypeText().replace(/[\/\\:*?"<>|]/g, '_');

    await new Promise(resolve => setTimeout(resolve, 300));

    html2pdf().set({
        filename: cleanFileName + ".pdf",
        html2canvas: {
            scale: 3,
            useCORS: true,
            scrollY: 0,
            backgroundColor: '#ffffff',
            onclone: function(clonedDoc) {
                clonedDoc.getElementById('report-content').style.background = '#ffffff';
                clonedDoc.querySelectorAll('*').forEach(el => {
                    el.style.color = '';
                    el.style.backgroundColor = '';
                });
            }
        },
        jsPDF: {unit: "mm", format: "a4", orientation: "portrait"}
    })
    .from(reportContent)
    .save()
    .then(() => {
        document.querySelector('.control-bar').style.visibility = 'visible';
        document.querySelector('.top-marquee').style.visibility = 'visible';
        document.body.style.margin = "";
        document.body.style.background = "#f9fcfb";
        reportContent.style.display = 'none';
        showNotification("تم تنزيل التقرير بصيغة PDF ✓");
    });
}

async function sharePDFWhatsApp(){
    document.querySelector('.control-bar').style.visibility = 'hidden';
    document.querySelector('.top-marquee').style.visibility = 'visible';
    document.body.style.margin = "0";
    document.body.style.background = "white";

    const reportContent = document.getElementById('report-content');
    reportContent.style.display = 'block';
    reportContent.style.visibility = 'visible';
    reportContent.style.opacity = '1';
    reportContent.style.position = 'relative';
    reportContent.style.top = '0';
    reportContent.style.left = '0';

    const reportName = getReportTypeText();

    await new Promise(resolve => setTimeout(resolve, 300));

    await html2pdf().set({
        margin: 0,
        image: {type: "jpeg", quality: 1},
        html2canvas: {
            scale: 3,
            scrollY: 0,
            useCORS: true,
            backgroundColor: '#ffffff',
            onclone: function(clonedDoc) {
                clonedDoc.getElementById('report-content').style.background = '#ffffff';
            }
        },
        jsPDF: {unit: "mm", format: "a4", orientation: "portrait"}
    })
    .from(reportContent)
    .toPdf()
    .output('blob')
    .then((pdfBlob) => {
        document.querySelector('.control-bar').style.visibility = 'visible';
        document.querySelector('.top-marquee').style.visibility = 'visible';
        document.body.style.margin = "";
        document.body.style.background = "#f9fcfb";
        reportContent.style.display = 'none';

        let file = new File([pdfBlob], reportName + ".pdf", {type: "application/pdf"});
        if(navigator.canShare && navigator.canShare({files:[file]})){
            navigator.share({
                files:[file], 
                title: reportName,
                text: "تقرير: " + reportName
            });
        } else {
            let url = URL.createObjectURL(pdfBlob);
            window.open(`https://wa.me/?text=${encodeURIComponent("تقرير: " + reportName + "\n\n" + url)}`, "_blank");
        }
    });
}

// دالة تحميل التواريخ
async function loadDates(){
    const savedData = localStorage.getItem('teacherData');
    if (savedData) {
        const teacherData = JSON.parse(savedData);
        if (teacherData.manualHijriDate) {
            currentHijriDate = teacherData.manualHijriDate;
            currentGregorianDate = teacherData.manualGregorianDate || '';
            document.getElementById('manualDateInput').value = currentHijriDate;
            document.getElementById('currentDateDisplay').textContent = `هجري: ${currentHijriDate}`;
            document.getElementById('hDate').innerHTML = currentHijriDate + " هـ";
            document.getElementById('gDate').innerHTML = currentGregorianDate ? currentGregorianDate + " م" : currentHijriDate + " هـ";
            return;
        }
    }
    
    let g = new Date();
    currentGregorianDate = `${g.getDate()}/${g.getMonth()+1}/${g.getFullYear()}`;
    
    try {
        let r = await fetch(`https://api.aladhan.com/v1/gToH?date=${g.getDate()}-${g.getMonth()+1}-${g.getFullYear()}`);
        let j = await r.json();
        let h = j.data.hijri;
        
        const englishToArabic = {
            '0': '٠', '1': '١', '2': '٢', '3': '٣', '4': '٤',
            '5': '٥', '6': '٦', '7': '٧', '8': '٨', '9': '٩'
        };
        
        let arabicDay = h.day.toString();
        let arabicMonth = h.month.number.toString();
        let arabicYear = h.year.toString();
        
        for (let english in englishToArabic) {
            arabicDay = arabicDay.replace(new RegExp(english, 'g'), englishToArabic[english]);
            arabicMonth = arabicMonth.replace(new RegExp(english, 'g'), englishToArabic[english]);
            arabicYear = arabicYear.replace(new RegExp(english, 'g'), englishToArabic[english]);
        }
        
        currentHijriDate = `${arabicYear}/${arabicMonth}/${arabicDay}`;
        
        document.getElementById('manualDateInput').value = currentHijriDate;
        document.getElementById('currentDateDisplay').textContent = `هجري: ${currentHijriDate}`;
        
        document.getElementById('gDate').innerHTML = currentGregorianDate + " م";
        document.getElementById('hDate').innerHTML = currentHijriDate + " هـ";
    } catch {
        currentHijriDate = "١٤٤٦/٠٦/٠١";
        document.getElementById('currentDateDisplay').textContent = "تعذر تحميل التاريخ";
        document.getElementById('manualDateInput').value = currentHijriDate;
        document.getElementById('gDate').innerHTML = currentGregorianDate + " م";
        document.getElementById('hDate').innerHTML = currentHijriDate + " هجري";
    }
}

// عند تحميل الصفحة
window.onload = function() {
    loadDates();
    loadTeacherData();
    loadKeysFromStorage(); // تحميل المفاتيح المحفوظة
    updateReport();
    
    // إضافة مستمع البحث
    document.getElementById('reportSearch').addEventListener('input', handleReportSearch);
    
    // إخفاء نتائج البحث عند النقر خارجها
    document.addEventListener('click', function(event) {
        const searchResults = document.getElementById('searchResults');
        const reportSearch = document.getElementById('reportSearch');
        
        if (!event.target.closest('#reportSearchContainer')) {
            searchResults.style.display = 'none';
        }
    });
    
    // تحسينات للهواتف
    if ('ontouchstart' in window) {
        document.body.classList.add('touch-device');
    }
    
    document.querySelectorAll('button').forEach(btn => {
        btn.addEventListener('touchstart', function() {
            this.style.transform = 'scale(0.95)';
        });
        btn.addEventListener('touchend', function() {
            this.style.transform = '';
        });
    });
    
    // تحسين البحث للمس
    document.addEventListener('click', function(e) {
        if (e.target.closest('#searchResults div')) {
            const clickedReport = e.target.textContent;
            const reportTypeSelect = document.getElementById('reportType');
            const categorySelect = document.getElementById('reportCategory');
            
            for (const category in allReportsByCategory) {
                if (allReportsByCategory[category].includes(clickedReport)) {
                    categorySelect.value = category;
                    const reports = allReportsByCategory[category] || [];
                    updateReportTypeOptions(reports);
                    break;
                }
            }
            
            reportTypeSelect.value = clickedReport;
            handleReportType();
            updateReport();
            document.getElementById('searchResults').style.display = 'none';
            document.getElementById('reportSearch').value = '';
        }
    });
    
    // تحسينات للأندرويد
    if (navigator.userAgent.match(/Android/i)) {
        document.body.classList.add('android-fix');
    }
    
    // إصلاح مشكلة التمرير على iOS
    document.addEventListener('touchmove', function(e) {
        if (e.target.type === 'range') {
            e.preventDefault();
        }
    }, { passive: false });
    
    // إضافة مؤشر التحميل لزر الذكاء الاصطناعي
    const aiButton = document.getElementById('aiFillBtn');
    const aiButton2 = document.getElementById('aiFillBtn2');
    const loadingIndicator = document.createElement('div');
    loadingIndicator.className = 'ai-loading-indicator';
    aiButton.appendChild(loadingIndicator.cloneNode(true));
    aiButton2.appendChild(loadingIndicator.cloneNode(true));
    
    // إضافة مستمعات لأحداث تكيف الخطوط
    window.addEventListener('resize', adaptSubjectLessonFont);
    
    document.addEventListener('input', function(e) {
        if (e.target.id === 'subject' || e.target.id === 'lesson') {
            setTimeout(adaptSubjectLessonFont, 50);
        }
    });
    
    document.addEventListener('change', function(e) {
        if (e.target.matches('select, input[type="text"], input[type="file"]')) {
            setTimeout(adaptSubjectLessonFont, 100);
        }
    });
}
</script>

</body>
</html>