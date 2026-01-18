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
    flex