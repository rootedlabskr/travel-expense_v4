[travel_expense_tracker.html](https://github.com/user-attachments/files/28752002/travel_expense_tracker.html)
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>여행 회계 정리</title>
<style>
  :root {
    --bg: #F7F9FC;
    --surface: #FFFFFF;
    --surface2: #EEF2F8;
    --primary: #2C5EE8;
    --primary-light: #E8EEFB;
    --personal: #E05C2A;
    --personal-light: #FDF0EB;
    --shared: #2C5EE8;
    --shared-light: #E8EEFB;
    --green: #1DA462;
    --green-light: #E6F6EE;
    --text: #1A1F2E;
    --text2: #5A6478;
    --text3: #9AA3B5;
    --border: #DDE3ED;
    --shadow: 0 2px 12px rgba(44,94,232,0.08);
    --radius: 12px;
    --radius-sm: 8px;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Pretendard', 'Apple SD Gothic Neo', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    font-size: 14px;
  }

  /* Header */
  .header {
    background: var(--primary);
    color: white;
    padding: 20px 24px 16px;
    position: sticky;
    top: 0;
    z-index: 100;
    box-shadow: 0 2px 16px rgba(44,94,232,0.3);
  }
  .header h1 { font-size: 20px; font-weight: 700; letter-spacing: -0.3px; }
  .header p { font-size: 12px; opacity: 0.8; margin-top: 2px; }

  /* Tabs */
  .tabs {
    display: flex;
    background: var(--surface);
    border-bottom: 1px solid var(--border);
    position: sticky;
    top: 64px;
    z-index: 99;
  }
  .tab {
    flex: 1;
    padding: 14px 8px;
    text-align: center;
    font-size: 13px;
    font-weight: 600;
    color: var(--text3);
    cursor: pointer;
    border-bottom: 2px solid transparent;
    transition: all 0.2s;
  }
  .tab.active { color: var(--primary); border-bottom-color: var(--primary); }

  /* Main content */
  .main { padding: 16px; padding-bottom: 48px; max-width: 600px; margin: 0 auto; }

  /* Cards */
  .card {
    background: var(--surface);
    border-radius: var(--radius);
    padding: 16px;
    margin-bottom: 12px;
    box-shadow: var(--shadow);
  }
  .card-title {
    font-size: 13px;
    font-weight: 700;
    color: var(--text2);
    text-transform: uppercase;
    letter-spacing: 0.5px;
    margin-bottom: 12px;
  }

  /* Summary bar */
  .summary-grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    gap: 10px;
    margin-bottom: 12px;
  }
  .summary-item {
    background: var(--surface);
    border-radius: var(--radius-sm);
    padding: 12px;
    text-align: center;
    box-shadow: var(--shadow);
  }
  .summary-item .label { font-size: 11px; color: var(--text3); font-weight: 600; }
  .summary-item .amount { font-size: 16px; font-weight: 800; margin-top: 4px; }
  .summary-item.total .amount { color: var(--text); }
  .summary-item.shared .amount { color: var(--shared); }
  .summary-item.personal .amount { color: var(--personal); }

  /* Form */
  .form-group { margin-bottom: 12px; }
  .form-label { display: block; font-size: 12px; font-weight: 600; color: var(--text2); margin-bottom: 5px; }
  .form-input {
    width: 100%;
    padding: 10px 12px;
    border: 1.5px solid var(--border);
    border-radius: var(--radius-sm);
    font-size: 14px;
    color: var(--text);
    background: var(--bg);
    transition: border-color 0.2s;
    outline: none;
  }
  .form-input:focus { border-color: var(--primary); background: white; }
  .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  
  /* Type toggle */
  .type-toggle { display: flex; gap: 8px; }
  .type-btn {
    flex: 1;
    padding: 10px;
    border: 1.5px solid var(--border);
    border-radius: var(--radius-sm);
    background: var(--bg);
    font-size: 13px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s;
    color: var(--text2);
  }
  .type-btn.shared.active { background: var(--shared-light); border-color: var(--shared); color: var(--shared); }
  .type-btn.personal.active { background: var(--personal-light); border-color: var(--personal); color: var(--personal); }

  /* Buttons */
  .btn {
    padding: 12px 20px;
    border: none;
    border-radius: var(--radius-sm);
    font-size: 14px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.15s;
    width: 100%;
  }
  .btn-primary { background: var(--primary); color: white; }
  .btn-primary:hover { background: #1e4dd4; }
  .btn-danger { background: #fee; color: #c0392b; border: 1px solid #fcc; font-size: 12px; padding: 6px 10px; width: auto; }
  .btn-sm { font-size: 12px; padding: 6px 12px; width: auto; }
  .btn-outline { background: transparent; border: 1.5px solid var(--border); color: var(--text2); }

  /* Member chips */
  .member-chips { display: flex; flex-wrap: wrap; gap: 6px; }
  .chip {
    display: inline-flex;
    align-items: center;
    gap: 5px;
    padding: 5px 10px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 600;
    background: var(--primary-light);
    color: var(--primary);
  }
  .chip .remove {
    cursor: pointer;
    width: 14px;
    height: 14px;
    border-radius: 50%;
    background: rgba(44,94,232,0.2);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 10px;
    font-weight: 800;
  }
  .add-member-row { display: flex; gap: 8px; margin-top: 10px; }

  /* Expense list */
  .expense-item {
    display: flex;
    align-items: center;
    padding: 12px 0;
    border-bottom: 1px solid var(--border);
    gap: 10px;
  }
  .expense-item:last-child { border-bottom: none; }
  .exp-badge {
    width: 34px;
    height: 34px;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 16px;
    flex-shrink: 0;
  }
  .exp-badge.shared { background: var(--shared-light); }
  .exp-badge.personal { background: var(--personal-light); }
  .exp-info { flex: 1; min-width: 0; }
  .exp-name { font-size: 14px; font-weight: 700; }
  .exp-meta { font-size: 11px; color: var(--text3); margin-top: 2px; }
  .exp-amount { font-size: 15px; font-weight: 800; text-align: right; }
  .exp-amount.shared { color: var(--shared); }
  .exp-amount.personal { color: var(--personal); }

  /* Settlement */
  .settlement-item {
    background: var(--green-light);
    border-radius: var(--radius-sm);
    padding: 12px 14px;
    margin-bottom: 8px;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .settlement-item .arrow { color: var(--green); font-size: 16px; }
  .settlement-detail { flex: 1; font-size: 13px; }
  .settlement-detail strong { color: var(--text); }
  .settlement-amount { font-size: 15px; font-weight: 800; color: var(--green); }

  .person-balance {
    background: var(--surface2);
    border-radius: var(--radius-sm);
    padding: 10px 14px;
    margin-bottom: 6px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .person-balance .name { font-size: 13px; font-weight: 600; }
  .person-balance .bal { font-size: 13px; font-weight: 800; }
  .bal.pos { color: var(--green); }
  .bal.neg { color: var(--personal); }
  .bal.zero { color: var(--text3); }

  /* Filter tabs */
  .filter-tabs { display: flex; gap: 6px; margin-bottom: 12px; }
  .filter-tab {
    padding: 6px 12px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 600;
    cursor: pointer;
    border: 1.5px solid var(--border);
    background: var(--bg);
    color: var(--text2);
    transition: all 0.2s;
  }
  .filter-tab.active { background: var(--primary); border-color: var(--primary); color: white; }

  .empty { text-align: center; padding: 30px; color: var(--text3); font-size: 13px; }
  .empty .icon { font-size: 36px; margin-bottom: 8px; }

  /* Section header */
  .section-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 8px;
  }
  .section-header .section-title { font-size: 14px; font-weight: 700; }

  /* Export */
  .export-btn {
    background: var(--green-light);
    color: var(--green);
    border: 1.5px solid #9de3bc;
    padding: 10px 16px;
    border-radius: var(--radius-sm);
    font-size: 13px;
    font-weight: 700;
    cursor: pointer;
    width: 100%;
    margin-top: 8px;
  }

  .page { display: none; }
  .page.active { display: block; }

  /* Receipt scan styles */
  .scan-zone {
    border: 2px dashed var(--border);
    border-radius: var(--radius);
    padding: 28px 16px;
    text-align: center;
    cursor: pointer;
    transition: all 0.2s;
    background: var(--bg);
    position: relative;
  }
  .scan-zone:hover, .scan-zone.dragover {
    border-color: var(--primary);
    background: var(--primary-light);
  }
  .scan-zone .scan-icon { font-size: 40px; margin-bottom: 8px; }
  .scan-zone .scan-label { font-size: 14px; font-weight: 700; color: var(--text2); }
  .scan-zone .scan-sub { font-size: 12px; color: var(--text3); margin-top: 4px; }
  .scan-zone input[type=file] { display: none; }

  .scan-preview {
    width: 100%;
    max-height: 200px;
    object-fit: contain;
    border-radius: var(--radius-sm);
    margin: 10px 0;
    border: 1px solid var(--border);
  }

  .scan-loading {
    display: flex;
    align-items: center;
    gap: 10px;
    background: var(--primary-light);
    border-radius: var(--radius-sm);
    padding: 14px;
    margin: 10px 0;
    font-size: 13px;
    color: var(--primary);
    font-weight: 600;
  }
  .spinner {
    width: 18px; height: 18px;
    border: 2px solid rgba(44,94,232,0.3);
    border-top-color: var(--primary);
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
    flex-shrink: 0;
  }
  @keyframes spin { to { transform: rotate(360deg); } }

  .scan-result {
    background: var(--green-light);
    border: 1.5px solid #9de3bc;
    border-radius: var(--radius-sm);
    padding: 14px;
    margin: 10px 0;
  }
  .scan-result .result-title {
    font-size: 12px;
    font-weight: 700;
    color: var(--green);
    margin-bottom: 8px;
    display: flex;
    align-items: center;
    gap: 5px;
  }
  .scan-result-items { display: flex; flex-direction: column; gap: 6px; }
  .scan-result-item {
    background: white;
    border-radius: 6px;
    padding: 10px 12px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
    border: 1.5px solid transparent;
    transition: all 0.15s;
  }
  .scan-result-item:hover { border-color: var(--primary); }
  .scan-result-item.selected { border-color: var(--primary); background: var(--primary-light); }
  .scan-result-item .item-name { font-size: 13px; font-weight: 600; }
  .scan-result-item .item-amt { font-size: 14px; font-weight: 800; color: var(--primary); }
  .scan-result-item .item-date { font-size: 11px; color: var(--text3); }

  .btn-scan-apply {
    background: var(--primary);
    color: white;
    border: none;
    border-radius: var(--radius-sm);
    padding: 11px 16px;
    font-size: 13px;
    font-weight: 700;
    cursor: pointer;
    width: 100%;
    margin-top: 8px;
  }
  .btn-scan-apply:disabled { opacity: 0.5; cursor: not-allowed; }

  .scan-error {
    background: #fff0f0;
    border: 1.5px solid #fcc;
    border-radius: var(--radius-sm);
    padding: 12px;
    font-size: 12px;
    color: #c0392b;
    margin: 10px 0;
  }

  .multi-item-list { display: flex; flex-direction: column; gap: 6px; max-height: 240px; overflow-y: auto; }

  /* Archive */
  .archive-item {
    background: var(--surface2);
    border-radius: var(--radius-sm);
    padding: 14px;
    margin-bottom: 10px;
    cursor: pointer;
    transition: all 0.15s;
    border: 1.5px solid transparent;
  }
  .archive-item:hover { border-color: var(--primary); background: var(--primary-light); }
  .archive-item .arc-name { font-size: 15px; font-weight: 700; }
  .archive-item .arc-meta { font-size: 12px; color: var(--text3); margin-top:4px; }
  .archive-item .arc-stats { display:flex; gap:12px; margin-top:8px; }
  .archive-item .arc-stat { font-size:12px; font-weight:600; }
  .archive-item .arc-stat span { color:var(--text3); font-weight:400; }
  .archive-actions { display:flex; gap:6px; margin-top:10px; }

  /* Save status bar */
  .save-bar {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(26,31,46,0.92);
    color: rgba(255,255,255,0.8);
    font-size: 11px;
    padding: 6px 16px;
    display: flex;
    align-items: center;
    gap: 6px;
    z-index: 998;
    backdrop-filter: blur(6px);
  }
  .save-bar .dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: var(--green);
    flex-shrink: 0;
  }

  /* Toast */
  .toast {
    position: fixed;
    bottom: 24px;
    left: 50%;
    transform: translateX(-50%) translateY(80px);
    background: #1A1F2E;
    color: white;
    padding: 12px 20px;
    border-radius: 24px;
    font-size: 13px;
    font-weight: 600;
    z-index: 999;
    transition: transform 0.3s ease;
    white-space: nowrap;
    box-shadow: 0 4px 20px rgba(0,0,0,0.3);
  }
  .toast.show { transform: translateX(-50%) translateY(0); }

  .notice {
    background: var(--primary-light);
    border-left: 3px solid var(--primary);
    padding: 10px 12px;
    border-radius: 0 var(--radius-sm) var(--radius-sm) 0;
    font-size: 12px;
    color: var(--primary);
    margin-bottom: 12px;
  }
</style>
</head>
<body>

<div class="header">
  <h1>✈️ 여행 회계 정리</h1>
  <p id="tripInfo">멤버를 추가하고 지출을 기록하세요</p>
</div>

<div class="tabs">
  <div class="tab active" onclick="switchTab('members')">👥 멤버</div>
  <div class="tab" onclick="switchTab('add')">＋ 지출 입력</div>
  <div class="tab" onclick="switchTab('list')">📋 내역</div>
  <div class="tab" onclick="switchTab('settle')">💰 정산</div>
  <div class="tab" onclick="switchTab('archive')">🗂 아카이브</div>
</div>

<div class="main">

  <!-- 요약 항상 표시 -->
  <div class="summary-grid" id="summaryBar">
    <div class="summary-item total">
      <div class="label">총 지출</div>
      <div class="amount" id="totalAmt">0원</div>
    </div>
    <div class="summary-item shared">
      <div class="label">🤝 공동</div>
      <div class="amount" id="sharedAmt">0원</div>
    </div>
    <div class="summary-item personal">
      <div class="label">👤 개인</div>
      <div class="amount" id="personalAmt">0원</div>
    </div>
  </div>

  <!-- 멤버 관리 탭 -->
  <div class="page active" id="page-members">
    <div class="card">
      <div class="card-title">여행 멤버 관리</div>
      <div class="member-chips" id="memberChips">
        <div class="empty" style="width:100%; padding:10px 0">
          <div>멤버를 추가해주세요</div>
        </div>
      </div>
      <div class="add-member-row">
        <input class="form-input" id="newMemberName" placeholder="이름 입력" style="flex:1" onkeypress="if(event.key==='Enter') addMember()">
        <button class="btn btn-primary btn-sm" onclick="addMember()">추가</button>
      </div>
    </div>

    <div class="card">
      <div class="card-title">여행 정보 (선택)</div>
      <div class="form-group">
        <label class="form-label">여행 이름</label>
        <input class="form-input" id="tripName" placeholder="예: 일본 여행 2025" oninput="updateTripInfo()">
      </div>
      <div class="form-row">
        <div class="form-group">
          <label class="form-label">출발일</label>
          <input class="form-input" id="tripStart" type="date" oninput="updateTripInfo()">
        </div>
        <div class="form-group">
          <label class="form-label">도착일</label>
          <input class="form-input" id="tripEnd" type="date" oninput="updateTripInfo()">
        </div>
      </div>
      <div class="form-group">
        <label class="form-label">여행 국가 / 통화</label>
        <select class="form-input" id="currencySelect" onchange="updateCurrency()">
          <option value="KRW">🇰🇷 한국 (₩ 원)</option>
          <option value="JPY">🇯🇵 일본 (¥ 엔)</option>
          <option value="USD">🇺🇸 미국 ($  달러)</option>
          <option value="EUR">🇪🇺 유럽 (€ 유로)</option>
          <option value="CNY">🇨🇳 중국 (¥ 위안)</option>
          <option value="THB">🇹🇭 태국 (฿ 바트)</option>
          <option value="VND">🇻🇳 베트남 (₫ 동)</option>
          <option value="TWD">🇹🇼 대만 (NT$ 달러)</option>
          <option value="HKD">🇭🇰 홍콩 (HK$ 달러)</option>
          <option value="GBP">🇬🇧 영국 (£ 파운드)</option>
          <option value="AUD">🇦🇺 호주 (A$ 달러)</option>
          <option value="SGD">🇸🇬 싱가포르 (S$ 달러)</option>
          <option value="MYR">🇲🇾 말레이시아 (RM 링깃)</option>
          <option value="PHP">🇵🇭 필리핀 (₱ 페소)</option>
        </select>
      </div>
    </div>

    <div class="card">
      <div class="card-title">🔑 AI 영수증 인식 설정</div>
      <div class="form-group">
        <label class="form-label">Anthropic API 키</label>
        <div style="display:flex;gap:8px">
          <input class="form-input" id="apiKeyInput" type="password"
            placeholder="sk-ant-..." style="flex:1"
            oninput="saveApiKey()">
          <button class="btn btn-primary btn-sm" onclick="toggleApiKeyVisibility()">👁</button>
        </div>
      </div>
      <div id="apiKeyStatus" style="font-size:12px;margin-top:4px"></div>
      <div style="font-size:11px;color:var(--text3);margin-top:6px">
        🔒 키는 이 기기 브라우저에만 저장되며 외부로 전송되지 않아요
      </div>
    </div>

    <div class="card">
      <div class="card-title">✈️ 여행 관리</div>
      <div style="font-size:13px;color:var(--text2);margin-bottom:12px">
        여행이 끝나면 아카이브에 보관하고 새 여행을 시작할 수 있어요.
      </div>
      <button class="btn btn-primary" onclick="endTrip()" style="background:var(--green)">
        ✅ 이 여행 종료하고 아카이브에 보관
      </button>
    </div>
  </div>

  <!-- 아카이브 탭 -->
  <div class="page" id="page-archive">
    <div class="card">
      <div class="card-title">🗂 지난 여행 목록</div>
      <div id="archiveList">
        <div class="empty"><div class="icon">🗂</div><div>아직 보관된 여행이 없어요</div></div>
      </div>
    </div>
  </div>

  <!-- 영수증 스캔 탭 (제거 - 직접입력에 통합) -->

  <!-- 지출 입력 탭 -->
  <div class="page" id="page-add">

    <!-- 영수증 업로드 -->
    <div class="card" id="scanCard">
      <div class="card-title">📷 영수증으로 자동 입력</div>
      <div class="scan-zone" id="scanZone" onclick="document.getElementById('receiptFile').click()"
           ondragover="event.preventDefault();this.classList.add('dragover')"
           ondragleave="this.classList.remove('dragover')"
           ondrop="handleDrop(event)">
        <div id="scanPreviewWrap" style="display:none">
          <img id="scanPreview" class="scan-preview" src="" alt="영수증 미리보기">
        </div>
        <div id="scanPlaceholder">
          <div class="scan-icon">📷</div>
          <div class="scan-label">영수증 사진을 올려주세요</div>
          <div class="scan-sub">탭하여 촬영하거나 파일 선택 · AI가 자동으로 채워드려요</div>
        </div>
        <input type="file" id="receiptFile" accept="image/*" capture="environment" onchange="handleReceiptFile(event)">
      </div>
      <div id="scanStatus"></div>

      <!-- 다중 항목 선택 UI (영수증 인식 후 표시) -->
      <div id="multiItemArea" style="display:none">
        <div class="scan-result">
          <div class="result-title" style="justify-content:space-between">
            <span>✅ 추가할 항목 선택</span>
            <span onclick="toggleSelectAll()" style="cursor:pointer;font-size:12px;color:var(--primary)">전체선택</span>
          </div>
          <div class="multi-item-list" id="scanItems"></div>
        </div>
        <div class="form-group" style="margin-top:10px">
          <label class="form-label">납부자</label>
          <select class="form-input" id="scanPayer"></select>
        </div>
        <div class="form-group">
          <label class="form-label">지출 유형</label>
          <div class="type-toggle">
            <button class="type-btn shared active" id="scanTypeShared" onclick="setScanType('shared')">🤝 공동</button>
            <button class="type-btn personal" id="scanTypePersonal" onclick="setScanType('personal')">👤 개인</button>
          </div>
        </div>
        <div class="form-group" id="scanPersonalGroup" style="display:none">
          <label class="form-label">해당 멤버</label>
          <select class="form-input" id="scanPersonalMember"></select>
        </div>
        <button class="btn-scan-apply" onclick="addAllSelectedItems()" style="margin-top:8px">📥 선택 항목 저장하기</button>
      </div>
    </div>

    <!-- 구분선 -->
    <div style="display:flex;align-items:center;gap:10px;margin:4px 0" id="dividerManual">
      <div style="flex:1;height:1px;background:var(--border)"></div>
      <div style="font-size:12px;color:var(--text3);font-weight:600">또는 직접 입력</div>
      <div style="flex:1;height:1px;background:var(--border)"></div>
    </div>

    <!-- 수동 입력 폼 -->
    <div class="card" id="formCard">
      <div class="card-title">✏️ 지출 직접 입력</div>
      <div id="autofillBadge" style="display:none;background:var(--green-light);border-left:3px solid var(--green);padding:10px 12px;border-radius:0 var(--radius-sm) var(--radius-sm) 0;font-size:12px;color:var(--green);margin-bottom:12px">
        📷 영수증에서 자동 입력됐어요. 확인 후 저장해주세요.
      </div>
      <div class="form-group">
        <label class="form-label">지출 유형</label>
        <div class="type-toggle">
          <button class="type-btn shared active" id="typeShared" onclick="setType('shared')">🤝 공동 지출</button>
          <button class="type-btn personal" id="typePersonal" onclick="setType('personal')">👤 개인 지출</button>
        </div>
      </div>
      <div class="form-group">
        <label class="form-label">항목명</label>
        <input class="form-input" id="expName" placeholder="예: 저녁 식사, 택시, 숙박">
      </div>
      <div class="form-row">
        <div class="form-group">
          <label class="form-label">금액</label>
          <input class="form-input" id="expAmount" type="number" placeholder="0" min="0">
        </div>
        <div class="form-group">
          <label class="form-label">날짜</label>
          <input class="form-input" id="expDate" type="date">
        </div>
      </div>
      <div class="form-group">
        <label class="form-label">납부자</label>
        <select class="form-input" id="expPayer">
          <option value="">-- 멤버를 먼저 추가해주세요 --</option>
        </select>
      </div>
      <div class="form-group" id="personalMemberGroup" style="display:none">
        <label class="form-label">해당 멤버 (개인 지출 시)</label>
        <select class="form-input" id="expPersonalMember">
          <option value="">-- 선택 --</option>
        </select>
      </div>
      <div class="form-group">
        <label class="form-label">메모 (선택)</label>
        <input class="form-input" id="expMemo" placeholder="추가 메모">
      </div>
      <button class="btn btn-primary" onclick="addExpense()">💾 저장하기</button>
    </div>
  </div>

  <!-- 내역 탭 -->
  <div class="page" id="page-list">
    <div class="filter-tabs">
      <div class="filter-tab active" onclick="setFilter('all')">전체</div>
      <div class="filter-tab" onclick="setFilter('shared')">🤝 공동</div>
      <div class="filter-tab" onclick="setFilter('personal')">👤 개인</div>
    </div>
    <div class="card">
      <div id="expenseList">
        <div class="empty"><div class="icon">📝</div><div>지출 내역이 없습니다</div></div>
      </div>
    </div>
    <button class="export-btn" onclick="exportCSV()">📥 CSV 백업 다운로드</button>
  </div>

  <!-- 정산 탭 -->
  <div class="page" id="page-settle">
    <div class="notice">공동 지출만 정산 대상입니다. 개인 지출은 본인 부담입니다.</div>
    <div class="card">
      <div class="card-title">멤버별 잔액 현황</div>
      <div id="balanceList">
        <div class="empty"><div>멤버와 지출을 추가해주세요</div></div>
      </div>
    </div>
    <div class="card">
      <div class="card-title">정산 방법 (최소 이체)</div>
      <div id="settlementList">
        <div class="empty"><div>정산할 내역이 없습니다</div></div>
      </div>
    </div>
    <div class="card">
      <div class="card-title">개인별 개인 지출 현황</div>
      <div id="personalSummary">
        <div class="empty"><div>개인 지출이 없습니다</div></div>
      </div>
    </div>
  </div>

</div>

<div class="save-bar" id="saveBar">
  <div class="dot"></div>
  <span id="saveTime">저장된 데이터 없음</span>
</div>

<div class="toast" id="toast"></div>

<script>
let state = {
  members: [],
  expenses: [],
  expenseType: 'shared',
  filter: 'all',
  tripName: '',
  tripStart: '',
  tripEnd: '',
  currency: 'KRW',
};

const CURRENCIES = {
  KRW: { symbol: '₩', name: '원',     locale: 'ko-KR', decimals: 0 },
  JPY: { symbol: '¥', name: '엔',     locale: 'ja-JP', decimals: 0 },
  USD: { symbol: '$', name: '달러',   locale: 'en-US', decimals: 2 },
  EUR: { symbol: '€', name: '유로',   locale: 'de-DE', decimals: 2 },
  CNY: { symbol: '¥', name: '위안',   locale: 'zh-CN', decimals: 2 },
  THB: { symbol: '฿', name: '바트',   locale: 'th-TH', decimals: 2 },
  VND: { symbol: '₫', name: '동',     locale: 'vi-VN', decimals: 0 },
  TWD: { symbol: 'NT$', name: '달러', locale: 'zh-TW', decimals: 0 },
  HKD: { symbol: 'HK$', name: '달러',locale: 'zh-HK', decimals: 2 },
  GBP: { symbol: '£', name: '파운드', locale: 'en-GB', decimals: 2 },
  AUD: { symbol: 'A$', name: '달러',  locale: 'en-AU', decimals: 2 },
  SGD: { symbol: 'S$', name: '달러',  locale: 'en-SG', decimals: 2 },
  MYR: { symbol: 'RM', name: '링깃',  locale: 'ms-MY', decimals: 2 },
  PHP: { symbol: '₱', name: '페소',   locale: 'fil-PH', decimals: 2 },
};

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 3000);
}

function getCurrency() {
  return CURRENCIES[state.currency] || CURRENCIES['KRW'];
}

function updateCurrency() {
  state.currency = document.getElementById('currencySelect').value;
  updateSummary();
  updateTripInfo();
  saveState();
}

function fmt(n) {
  const c = getCurrency();
  const num = c.decimals === 0
    ? Math.round(n).toLocaleString(c.locale)
    : n.toLocaleString(c.locale, { minimumFractionDigits: c.decimals, maximumFractionDigits: c.decimals });
  return c.symbol + num;
}

// 오늘 날짜 기본값
document.getElementById('expDate').value = new Date().toISOString().split('T')[0];

function switchTab(tab) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  document.getElementById('page-' + tab).classList.add('active');
  const tabs = ['members','add','list','settle','archive'];
  document.querySelectorAll('.tab')[tabs.indexOf(tab)].classList.add('active');
  if (tab === 'settle') calcSettlement();
  if (tab === 'list') renderList();
  if (tab === 'archive') renderArchive();
}

// ── 영수증 스캔 (직접입력 폼 자동채우기) ────────────────

let scanState = { items: [], selected: new Set(), scanType: 'shared' };

function setScanType(type) {
  scanState.scanType = type;
  document.getElementById('scanTypeShared').classList.toggle('active', type === 'shared');
  document.getElementById('scanTypePersonal').classList.toggle('active', type === 'personal');
  document.getElementById('scanPersonalGroup').style.display = type === 'personal' ? 'block' : 'none';
}

function updateScanSelects() {
  const opts = state.members.length
    ? state.members.map(m => `<option value="${m}">${m}</option>`).join('')
    : '<option value="">-- 멤버를 먼저 추가해주세요 --</option>';
  document.getElementById('scanPayer').innerHTML = opts;
  document.getElementById('scanPersonalMember').innerHTML = `<option value="">-- 선택 --</option>` + opts;
}

function handleDrop(e) {
  e.preventDefault();
  document.getElementById('scanZone').classList.remove('dragover');
  const file = e.dataTransfer.files[0];
  if (file && file.type.startsWith('image/')) processReceiptFile(file);
}

function handleReceiptFile(e) {
  const file = e.target.files[0];
  if (file) processReceiptFile(file);
}

function processReceiptFile(file) {
  const reader = new FileReader();
  reader.onload = (e) => {
    const dataUrl = e.target.result;
    document.getElementById('scanPreview').src = dataUrl;
    document.getElementById('scanPreviewWrap').style.display = 'block';
    document.getElementById('scanPlaceholder').style.display = 'none';
    document.getElementById('multiItemArea').style.display = 'none';
    document.getElementById('autofillBadge').style.display = 'none';
    analyzeReceipt(dataUrl);
  };
  reader.onerror = () => {
    document.getElementById('scanStatus').innerHTML =
      `<div class="scan-error">❌ 파일을 읽을 수 없습니다. 다시 시도해주세요.</div>`;
  };
  reader.readAsDataURL(file);
}

async function analyzeReceipt(dataUrl) {
  const status = document.getElementById('scanStatus');

  const apiKey = getApiKey();
  if (!apiKey) {
    status.innerHTML = `<div class="scan-error">❌ API 키가 없어요! 멤버 탭 → 🔑 API 키 설정에서 입력해주세요</div>`;
    return;
  }

  status.innerHTML = `<div class="scan-loading"><div class="spinner"></div>영수증 분석 중... (5~10초 소요)</div>`;

  const base64 = dataUrl.split(',')[1];
  const mediaType = dataUrl.split(';')[0].split(':')[1] || 'image/jpeg';

  try {
    const response = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': apiKey,
        'anthropic-version': '2023-06-01',
        'anthropic-dangerous-direct-browser-access': 'true',
      },
      body: JSON.stringify({
        model: 'claude-sonnet-4-5',
        max_tokens: 1500,
        messages: [
          {
            role: 'user',
            content: [
              { type: 'image', source: { type: 'base64', media_type: mediaType, data: base64 } },
              { type: 'text', text: `당신은 영수증 OCR 전문가입니다. 이 영수증 이미지를 꼼꼼히 분석해서 JSON으로만 응답하세요. 마크다운이나 설명 없이 순수 JSON만 출력하세요.

분석 순서:
1. 영수증의 언어/국가를 파악해서 통화 코드 결정 (예: 일본어→JPY, 영어+$→USD, 한국어→KRW, 태국어→THB 등)
2. 가게/업체명 찾기
3. 날짜 찾기 (없으면 null)
4. 총 결제 금액 찾기 (합계, 総額, TOTAL, 결제금액 등) - 통화 기호/쉼표 제거하고 숫자만
5. 개별 항목들 찾기 (메뉴명+금액)

응답 형식:
{"currency":"통화코드(KRW/JPY/USD/EUR/CNY/THB/VND/TWD/HKD/GBP/AUD/SGD/MYR/PHP 중 하나)","store":"가게명또는null","date":"YYYY-MM-DD또는null","total":총금액숫자,"items":[{"name":"한국어번역 (원어)","amount":금액숫자}]}

중요 규칙:
- amount는 반드시 숫자(쉼표/통화기호 제거)
- name은 반드시 "한국어번역 (원어)" 형식으로 표기 (예: "페이스 크림 케이스 (ティクリームケースフェイス)", "치킨버거 (Chicken Burger)")
- 원어가 한국어인 경우 번역 없이 그냥 한국어만 표기
- 개별 항목이 보이면 items에 각각 추가
- 개별 항목 없이 합계만 있으면 items에 {"name":"가게명또는영수증지출","amount":합계금액} 하나만
- 영수증이 아닌 이미지면 {"currency":"KRW","store":null,"date":null,"total":0,"items":[]}` }
            ]
          }
        ]
      })
    });

    const data = await response.json();

    // API 오류 처리
    if (data.error) throw new Error(data.error.message || 'API 오류');

    const text = data.content.filter(b => b.type === 'text').map(b => b.text).join('');
    // JSON 추출 (혹시 앞뒤에 텍스트 있어도 처리)
    const jsonMatch = text.match(/\{[\s\S]*\}/);
    if (!jsonMatch) throw new Error('JSON 파싱 실패: ' + text.slice(0, 100));
    const parsed = JSON.parse(jsonMatch[0]);

    const today = new Date().toISOString().split('T')[0];

    // AI가 감지한 통화로 자동 전환
    if (parsed.currency && CURRENCIES[parsed.currency] && parsed.currency !== state.currency) {
      state.currency = parsed.currency;
      document.getElementById('currencySelect').value = parsed.currency;
      updateTripInfo();
      updateSummary();
      const c = CURRENCIES[parsed.currency];
      status.innerHTML = `<div style="color:var(--primary);font-size:12px;font-weight:600;padding:4px 0">💱 통화 자동 감지: ${parsed.currency} (${c.symbol} ${c.name})로 변경되었습니다</div>`;
    }

    scanState.items = (parsed.items || []).map(it => ({
      name: it.name || parsed.store || '영수증 지출',
      amount: Number(it.amount) || 0,
      date: it.date || parsed.date || today
    })).filter(it => it.amount > 0);

    if (scanState.items.length === 0) {
      if (parsed.total > 0) {
        scanState.items = [{ name: parsed.store || '영수증 지출', amount: parsed.total, date: parsed.date || today }];
      } else {
        throw new Error('금액을 인식하지 못했습니다. 영수증이 선명한지 확인해주세요.');
      }
    }

    status.innerHTML = `<div style="color:var(--green);font-size:12px;font-weight:600;padding:6px 0">✅ ${scanState.items.length}개 항목 인식 완료</div>`;

    if (scanState.items.length === 1) {
      fillSingleItem(scanState.items[0]);
      // 단일 항목은 폼에 채워서 확인 후 저장
      document.getElementById('formCard').style.display = 'block';
      document.getElementById('dividerManual').style.display = 'flex';
    } else {
      // 다중 항목은 선택 UI만 표시, 수동 폼 숨기기
      scanState.selected.clear();
      scanState.items.forEach((_, i) => scanState.selected.add(i));
      updateScanSelects();
      renderScanItems();
      document.getElementById('multiItemArea').style.display = 'block';
      document.getElementById('formCard').style.display = 'none';
      document.getElementById('dividerManual').style.display = 'none';
    }

  } catch (err) {
    status.innerHTML = `<div class="scan-error">
      ❌ 오류: ${err.message}<br>
      <span style="font-size:11px;margin-top:4px;display:block">
        브라우저 콘솔(F12)에서 자세한 내용을 확인하세요
      </span>
    </div>`;
    console.error('Receipt analysis error:', err);
  }
}

function fillSingleItem(item) {
  document.getElementById('expName').value = item.name;
  document.getElementById('expAmount').value = item.amount;
  document.getElementById('expDate').value = item.date;
  document.getElementById('expMemo').value = '영수증 자동입력';
  document.getElementById('autofillBadge').style.display = 'block';
  // 폼 카드로 스크롤
  document.getElementById('formCard').scrollIntoView({ behavior: 'smooth', block: 'start' });
}

function renderScanItems() {
  const el = document.getElementById('scanItems');
  el.innerHTML = scanState.items.map((it, i) => `
    <div class="scan-result-item ${scanState.selected.has(i) ? 'selected' : ''}" onclick="toggleScanItem(${i})">
      <div>
        <div class="item-name">${it.name}</div>
        <div class="item-date">${it.date}</div>
      </div>
      <div style="display:flex;align-items:center;gap:8px">
        <div class="item-amt">${fmt(it.amount)}</div>
        <span style="font-size:18px">${scanState.selected.has(i) ? '✅' : '⬜'}</span>
      </div>
    </div>`).join('');
}

function toggleScanItem(i) {
  if (scanState.selected.has(i)) scanState.selected.delete(i);
  else scanState.selected.add(i);
  renderScanItems();
}

function toggleSelectAll() {
  if (scanState.selected.size === scanState.items.length) {
    scanState.selected.clear();
  } else {
    scanState.items.forEach((_, i) => scanState.selected.add(i));
  }
  renderScanItems();
}

function fillFormFromScan() {
  const i = [...scanState.selected][0];
  if (i === undefined) return;
  fillSingleItem(scanState.items[i]);
  document.getElementById('multiItemArea').style.display = 'none';
}

async function addAllSelectedItems() {
  if (state.members.length === 0) return alert('멤버를 먼저 추가해주세요');
  if (scanState.selected.size === 0) return alert('추가할 항목을 선택해주세요');

  const payer = document.getElementById('scanPayer').value;
  if (!payer) return alert('납부자를 선택해주세요');

  const type = scanState.scanType;
  const personalMember = type === 'personal' ? document.getElementById('scanPersonalMember').value : null;
  if (type === 'personal' && !personalMember) return alert('개인 지출 대상 멤버를 선택해주세요');

  let count = 0;
  for (const i of scanState.selected) {
    const it = scanState.items[i];
    const expense = {
      id: Date.now() + i,
      type, name: it.name, amount: it.amount,
      date: it.date, payer,
      memo: '영수증 자동입력',
      personalMember: type === 'personal' ? personalMember : null
    };
    state.expenses.push(expense);
    saveToSheet(expense);
    count++;
  }

  updateSummary();
  resetScanArea();
  showToast(`✅ ${count}개 항목이 추가됐어요! Google Sheets에도 저장됐어요`);
}

// 폼 초기화 시 스캔 영역도 리셋
function resetScanArea() {
  document.getElementById('scanPreviewWrap').style.display = 'none';
  document.getElementById('scanPlaceholder').style.display = 'block';
  document.getElementById('scanStatus').innerHTML = '';
  document.getElementById('multiItemArea').style.display = 'none';
  document.getElementById('autofillBadge').style.display = 'none';
  document.getElementById('receiptFile').value = '';
  document.getElementById('formCard').style.display = 'block';
  document.getElementById('dividerManual').style.display = 'flex';
  scanState = { items: [], selected: new Set(), scanType: 'shared' };
}

function addMember() {
  const name = document.getElementById('newMemberName').value.trim();
  if (!name) return alert('이름을 입력해주세요');
  if (state.members.includes(name)) return alert('이미 있는 멤버입니다');
  state.members.push(name);
  document.getElementById('newMemberName').value = '';
  renderMembers();
  updateSelects();
  saveState();
}

function removeMember(name) {
  if (state.expenses.some(e => e.payer === name || e.personalMember === name)) {
    return alert(`${name}님의 지출 내역이 있어 삭제할 수 없습니다`);
  }
  state.members = state.members.filter(m => m !== name);
  renderMembers();
  updateSelects();
  saveState();
}

function renderMembers() {
  const c = document.getElementById('memberChips');
  if (state.members.length === 0) {
    c.innerHTML = '<div class="empty" style="width:100%;padding:10px 0">멤버를 추가해주세요</div>';
    return;
  }
  c.innerHTML = state.members.map(m =>
    `<div class="chip">${m}<span class="remove" onclick="removeMember('${m}')">×</span></div>`
  ).join('');
}

function updateSelects() {
  const opts = state.members.length
    ? state.members.map(m => `<option value="${m}">${m}</option>`).join('')
    : '<option value="">-- 멤버를 먼저 추가해주세요 --</option>';
  document.getElementById('expPayer').innerHTML = opts;
  document.getElementById('expPersonalMember').innerHTML =
    `<option value="">-- 선택 --</option>` + opts;
}

function setType(type) {
  state.expenseType = type;
  document.getElementById('typeShared').classList.toggle('active', type === 'shared');
  document.getElementById('typePersonal').classList.toggle('active', type === 'personal');
  document.getElementById('personalMemberGroup').style.display =
    type === 'personal' ? 'block' : 'none';
}

// ── 여행 아카이브 ──────────────────────────────

function endTrip() {
  if (state.expenses.length === 0) return alert('지출 내역이 없어요. 지출을 추가한 후 종료해주세요.');
  const tripName = document.getElementById('tripName').value || '이름 없는 여행';
  if (!confirm(`"${tripName}" 여행을 종료하고 아카이브에 보관할까요?\n새 여행을 시작할 수 있어요.`)) return;

  // 현재 여행 아카이브에 저장
  const archives = JSON.parse(localStorage.getItem('travel_archives') || '[]');
  archives.unshift({
    id: Date.now(),
    name: tripName,
    start: document.getElementById('tripStart').value,
    end: document.getElementById('tripEnd').value,
    currency: state.currency,
    members: [...state.members],
    expenses: [...state.expenses],
    archivedAt: new Date().toISOString(),
  });
  localStorage.setItem('travel_archives', JSON.stringify(archives));

  // 현재 여행 초기화
  state.members = [];
  state.expenses = [];
  state.currency = 'KRW';
  document.getElementById('tripName').value = '';
  document.getElementById('tripStart').value = '';
  document.getElementById('tripEnd').value = '';
  document.getElementById('currencySelect').value = 'KRW';

  renderMembers();
  updateSelects();
  updateSummary();
  updateTripInfo();
  saveState();

  showToast(`✅ "${tripName}" 아카이브에 보관됐어요!`);
  switchTab('archive');
}

function renderArchive() {
  const archives = JSON.parse(localStorage.getItem('travel_archives') || '[]');
  const el = document.getElementById('archiveList');
  if (archives.length === 0) {
    el.innerHTML = '<div class="empty"><div class="icon">🗂</div><div>아직 보관된 여행이 없어요</div></div>';
    return;
  }
  el.innerHTML = archives.map(a => {
    const total = a.expenses.reduce((s, e) => s + e.amount, 0);
    const shared = a.expenses.filter(e => e.type === 'shared').reduce((s, e) => s + e.amount, 0);
    const c = CURRENCIES[a.currency] || CURRENCIES['KRW'];
    const dateRange = a.start && a.end ? `${a.start} ~ ${a.end}` : '날짜 없음';
    const archivedDate = new Date(a.archivedAt).toLocaleDateString('ko-KR');
    return `
      <div class="archive-item">
        <div class="arc-name">✈️ ${a.name}</div>
        <div class="arc-meta">${dateRange} · ${a.members.join(', ')} · 보관일 ${archivedDate}</div>
        <div class="arc-stats">
          <div class="arc-stat">총 지출 <span>${c.symbol}${total.toLocaleString()}</span></div>
          <div class="arc-stat">공동 <span>${c.symbol}${shared.toLocaleString()}</span></div>
          <div class="arc-stat">지출 <span>${a.expenses.length}건</span></div>
        </div>
        <div class="archive-actions">
          <button class="btn btn-outline btn-sm" onclick="loadArchiveTrip(${a.id})">📂 불러오기</button>
          <button class="btn btn-danger" onclick="deleteArchiveTrip(${a.id})">🗑️ 삭제</button>
        </div>
      </div>`;
  }).join('');
}

function loadArchiveTrip(id) {
  if (!confirm('이 여행을 불러올까요? 현재 진행 중인 여행 데이터는 먼저 아카이브에 보관해주세요.')) return;
  const archives = JSON.parse(localStorage.getItem('travel_archives') || '[]');
  const trip = archives.find(a => a.id === id);
  if (!trip) return;

  state.members = trip.members;
  state.expenses = trip.expenses;
  state.currency = trip.currency;
  document.getElementById('tripName').value = trip.name;
  document.getElementById('tripStart').value = trip.start || '';
  document.getElementById('tripEnd').value = trip.end || '';
  document.getElementById('currencySelect').value = trip.currency;

  renderMembers();
  updateSelects();
  updateSummary();
  updateTripInfo();
  saveState();

  showToast(`📂 "${trip.name}" 불러왔어요!`);
  switchTab('members');
}

function deleteArchiveTrip(id) {
  if (!confirm('이 여행 기록을 삭제할까요? 복구할 수 없어요.')) return;
  const archives = JSON.parse(localStorage.getItem('travel_archives') || '[]');
  const filtered = archives.filter(a => a.id !== id);
  localStorage.setItem('travel_archives', JSON.stringify(filtered));
  renderArchive();
  showToast('🗑️ 삭제됐어요');
}
function saveApiKey() {
  const key = document.getElementById('apiKeyInput').value.trim();
  localStorage.setItem('anthropic_api_key', key);
  const status = document.getElementById('apiKeyStatus');
  if (key.startsWith('sk-ant-')) {
    status.innerHTML = '<span style="color:var(--green)">✅ API 키가 저장되었습니다</span>';
  } else if (key.length > 0) {
    status.innerHTML = '<span style="color:var(--personal)">⚠️ 올바른 키 형식이 아니에요 (sk-ant-로 시작해야 해요)</span>';
  } else {
    status.innerHTML = '';
  }
}

function toggleApiKeyVisibility() {
  const input = document.getElementById('apiKeyInput');
  input.type = input.type === 'password' ? 'text' : 'password';
}

function getApiKey() {
  return localStorage.getItem('anthropic_api_key') || '';
}

// 저장된 API 키 불러오기
function loadApiKey() {
  const key = getApiKey();
  if (key) {
    document.getElementById('apiKeyInput').value = key;
    document.getElementById('apiKeyStatus').innerHTML =
      '<span style="color:var(--green)">✅ 저장된 API 키가 있습니다</span>';
  }
}


const SHEET_URL = 'https://script.google.com/macros/s/AKfycbxY3Asdat6TFjYxaHSXNZDp_66vo_oRyH2VDpYwpg49TdXRp4-c3Ky8ksUf9V1cSUL6Kg/exec';

async function saveToSheet(expense) {
  try {
    await fetch(SHEET_URL, {
      method: 'POST',
      mode: 'no-cors',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        date: expense.date,
        type: expense.type,
        name: expense.name,
        amount: expense.amount,
        currency: state.currency,
        payer: expense.payer,
        memo: expense.memo || '',
        personalMember: expense.personalMember || '',
        tripName: document.getElementById('tripName').value || '여행',
        // 정산 시트용 데이터
        members: state.members,
        allExpenses: state.expenses,
      })
    });
    return true;
  } catch(err) {
    console.error('Sheets 저장 실패:', err);
    return false;
  }
}

function addExpense() {
  if (state.members.length === 0) return alert('멤버를 먼저 추가해주세요');
  const name = document.getElementById('expName').value.trim();
  const amount = parseInt(document.getElementById('expAmount').value);
  const date = document.getElementById('expDate').value;
  const payer = document.getElementById('expPayer').value;
  const memo = document.getElementById('expMemo').value.trim();
  const personalMember = document.getElementById('expPersonalMember').value;

  if (!name) return alert('항목명을 입력해주세요');
  if (!amount || amount <= 0) return alert('올바른 금액을 입력해주세요');
  if (!payer) return alert('납부자를 선택해주세요');
  if (state.expenseType === 'personal' && !personalMember) return alert('개인 지출 대상 멤버를 선택해주세요');

  const expense = {
    id: Date.now(),
    type: state.expenseType,
    name, amount, date, payer, memo,
    personalMember: state.expenseType === 'personal' ? personalMember : null
  };

  state.expenses.push(expense);

  // 폼 초기화
  document.getElementById('expName').value = '';
  document.getElementById('expAmount').value = '';
  document.getElementById('expMemo').value = '';
  document.getElementById('expDate').value = new Date().toISOString().split('T')[0];
  resetScanArea();

  updateSummary();

  // Google Sheets 저장 (백그라운드)
  saveToSheet(expense).then(ok => {
    if (ok) {
      showToast('✅ 저장 완료! Google Sheets에 기록됐어요');
    } else {
      showToast('⚠️ 앱에는 저장됐지만 Sheets 저장 실패');
    }
  });
}

function deleteExpense(id) {
  if (!confirm('이 항목을 삭제할까요?')) return;
  state.expenses = state.expenses.filter(e => e.id !== id);
  updateSummary();
  renderList();
  showToast('🗑️ 삭제되었습니다');
}

function editExpense(id) {
  const e = state.expenses.find(e => e.id === id);
  if (!e) return;

  // 지출 입력 탭으로 이동
  switchTab('add');

  // 폼에 기존 데이터 채우기
  setType(e.type);
  document.getElementById('expName').value = e.name;
  document.getElementById('expAmount').value = e.amount;
  document.getElementById('expDate').value = e.date;
  document.getElementById('expMemo').value = e.memo || '';

  // 납부자 선택
  const payerEl = document.getElementById('expPayer');
  setTimeout(() => {
    payerEl.value = e.payer;
    if (e.personalMember) {
      document.getElementById('expPersonalMember').value = e.personalMember;
    }
  }, 50);

  // 수정 모드 표시
  document.getElementById('autofillBadge').style.display = 'block';
  document.getElementById('autofillBadge').innerHTML = '✏️ 수정 중입니다. 내용 변경 후 저장하세요.';
  document.getElementById('autofillBadge').style.background = 'var(--primary-light)';
  document.getElementById('autofillBadge').style.borderLeftColor = 'var(--primary)';
  document.getElementById('autofillBadge').style.color = 'var(--primary)';

  // 기존 항목 삭제 (저장 시 새로 추가됨)
  state.expenses = state.expenses.filter(ex => ex.id !== id);
  updateSummary();

  // 폼으로 스크롤
  setTimeout(() => {
    document.getElementById('formCard').scrollIntoView({ behavior: 'smooth', block: 'start' });
  }, 100);
}

function updateSummary() {
  const total = state.expenses.reduce((s, e) => s + e.amount, 0);
  const shared = state.expenses.filter(e => e.type === 'shared').reduce((s, e) => s + e.amount, 0);
  const personal = state.expenses.filter(e => e.type === 'personal').reduce((s, e) => s + e.amount, 0);
  document.getElementById('totalAmt').textContent = fmt(total);
  document.getElementById('sharedAmt').textContent = fmt(shared);
  document.getElementById('personalAmt').textContent = fmt(personal);
  saveState(); // 변경될 때마다 자동 저장
}

function setFilter(f) {
  state.filter = f;
  document.querySelectorAll('.filter-tab').forEach((t, i) => {
    t.classList.toggle('active', ['all','shared','personal'][i] === f);
  });
  renderList();
}

function renderList() {
  const filtered = state.filter === 'all'
    ? state.expenses
    : state.expenses.filter(e => e.type === state.filter);

  const list = document.getElementById('expenseList');
  if (filtered.length === 0) {
    list.innerHTML = '<div class="empty"><div class="icon">📝</div><div>지출 내역이 없습니다</div></div>';
    return;
  }

  const sorted = [...filtered].sort((a, b) => b.date.localeCompare(a.date));
  list.innerHTML = sorted.map(e => `
    <div class="expense-item">
      <div class="exp-badge ${e.type}">${e.type === 'shared' ? '🤝' : '👤'}</div>
      <div class="exp-info">
        <div class="exp-name">${e.name}</div>
        <div class="exp-meta">
          ${e.date} · ${e.payer} 결제
          ${e.type === 'personal' ? ` · ${e.personalMember} 사용` : ''}
          ${e.memo ? ` · ${e.memo}` : ''}
        </div>
      </div>
      <div>
        <div class="exp-amount ${e.type}">${fmt(e.amount)}</div>
        <div style="text-align:right;margin-top:4px;display:flex;gap:4px;justify-content:flex-end">
          <button class="btn btn-outline btn-sm" onclick="editExpense(${e.id})">✏️ 수정</button>
          <button class="btn btn-danger" onclick="deleteExpense(${e.id})">🗑️</button>
        </div>
      </div>
    </div>
  `).join('');
}

function calcSettlement() {
  const n = state.members.length;
  if (n === 0) return;

  // 공동 지출 정산
  const sharedExp = state.expenses.filter(e => e.type === 'shared');
  const total = sharedExp.reduce((s, e) => s + e.amount, 0);
  const perPerson = Math.round(total / n);

  const paid = {};
  state.members.forEach(m => paid[m] = 0);
  sharedExp.forEach(e => { paid[e.payer] = (paid[e.payer] || 0) + e.amount; });

  const balance = {};
  state.members.forEach(m => balance[m] = (paid[m] || 0) - perPerson);

  // 잔액 표시
  const balEl = document.getElementById('balanceList');
  if (n === 0) {
    balEl.innerHTML = '<div class="empty">멤버를 추가해주세요</div>';
  } else {
    balEl.innerHTML = state.members.map(m => {
      const b = balance[m] || 0;
      const cls = b > 0 ? 'pos' : b < 0 ? 'neg' : 'zero';
      const label = b > 0 ? `+${fmt(b)} 받을 예정` : b < 0 ? `${fmt(Math.abs(b))} 줘야 함` : '정산 완료';
      return `
        <div class="person-balance">
          <div>
            <div class="name">${m}</div>
            <div style="font-size:11px;color:var(--text3)">결제 ${fmt(paid[m]||0)} / 1인 기준 ${fmt(perPerson)}</div>
          </div>
          <div class="bal ${cls}">${label}</div>
        </div>`;
    }).join('');
  }

  // 최소 이체 알고리즘
  const debtors = state.members.filter(m => balance[m] < 0).map(m => ({name: m, amt: -balance[m]}));
  const creditors = state.members.filter(m => balance[m] > 0).map(m => ({name: m, amt: balance[m]}));

  const txns = [];
  let i = 0, j = 0;
  const d = debtors.map(x => ({...x}));
  const c = creditors.map(x => ({...x}));
  while (i < d.length && j < c.length) {
    const amt = Math.min(d[i].amt, c[j].amt);
    if (amt > 0) txns.push({from: d[i].name, to: c[j].name, amt});
    d[i].amt -= amt; c[j].amt -= amt;
    if (d[i].amt <= 0) i++;
    if (c[j].amt <= 0) j++;
  }

  const settleEl = document.getElementById('settlementList');
  if (txns.length === 0) {
    settleEl.innerHTML = '<div class="empty">정산할 이체가 없습니다 🎉</div>';
  } else {
    settleEl.innerHTML = txns.map(t => `
      <div class="settlement-item">
        <div class="settlement-detail">
          <strong>${t.from}</strong> → <strong>${t.to}</strong> 에게
        </div>
        <div class="settlement-amount">${fmt(t.amt)}</div>
      </div>`).join('');
  }

  // 개인 지출 요약
  const personalExp = state.expenses.filter(e => e.type === 'personal');
  const personalEl = document.getElementById('personalSummary');
  if (personalExp.length === 0) {
    personalEl.innerHTML = '<div class="empty">개인 지출이 없습니다</div>';
  } else {
    const byMember = {};
    personalExp.forEach(e => {
      byMember[e.personalMember] = (byMember[e.personalMember] || 0) + e.amount;
    });
    personalEl.innerHTML = Object.entries(byMember).map(([m, amt]) =>
      `<div class="person-balance">
        <div class="name">${m}</div>
        <div class="bal" style="color:var(--personal)">${fmt(amt)}</div>
      </div>`
    ).join('');
  }
}

function updateTripInfo() {
  const name = document.getElementById('tripName').value;
  const start = document.getElementById('tripStart').value;
  const end = document.getElementById('tripEnd').value;
  const c = getCurrency();
  let info = name || '여행 회계';
  if (start && end) info += ` (${start} ~ ${end})`;
  info += ` · ${state.currency} ${c.name}`;
  document.querySelector('.header p').textContent = info;
  saveState();
}

function exportCSV() {
  if (state.expenses.length === 0) return alert('내보낼 데이터가 없습니다');
  const c = getCurrency();
  const headers = ['날짜', '유형', '항목', `금액(${state.currency})`, '납부자', '개인멤버', '메모'];
  const rows = state.expenses.map(e => [
    e.date, e.type === 'shared' ? '공동' : '개인', e.name, e.amount, e.payer,
    e.personalMember || '', e.memo
  ]);
  const csv = [headers, ...rows].map(r => r.map(v => `"${v}"`).join(',')).join('\n');
  const blob = new Blob(['\uFEFF' + csv], {type: 'text/csv;charset=utf-8'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a'); a.href = url;
  a.download = (document.getElementById('tripName').value || '여행회계') + '.csv';
  a.click(); URL.revokeObjectURL(url);
}

// ── localStorage 저장/불러오기 ──────────────────────────────

function saveState() {
  localStorage.setItem('travel_state', JSON.stringify({
    members: state.members,
    expenses: state.expenses,
    expenseType: state.expenseType,
    currency: state.currency,
    tripName: document.getElementById('tripName').value,
    tripStart: document.getElementById('tripStart').value,
    tripEnd: document.getElementById('tripEnd').value,
  }));

  // 저장 시간 표시
  const now = new Date();
  const timeStr = now.toLocaleTimeString('ko-KR', { hour: '2-digit', minute: '2-digit', second: '2-digit' });
  const saveEl = document.getElementById('saveTime');
  if (saveEl) saveEl.textContent = `자동 저장됨 · ${timeStr}`;
}

function loadState() {
  try {
    const saved = localStorage.getItem('travel_state');
    if (!saved) return;
    const data = JSON.parse(saved);

    state.members = data.members || [];
    state.expenses = data.expenses || [];
    state.currency = data.currency || 'KRW';

    // UI 반영
    if (data.tripName) document.getElementById('tripName').value = data.tripName;
    if (data.tripStart) document.getElementById('tripStart').value = data.tripStart;
    if (data.tripEnd) document.getElementById('tripEnd').value = data.tripEnd;
    if (data.currency) document.getElementById('currencySelect').value = data.currency;

    renderMembers();
    updateSelects();
    updateSummary();
    updateTripInfo();

    if (state.expenses.length > 0) {
      showToast(`📂 저장된 데이터 불러왔어요 (지출 ${state.expenses.length}건)`);
    }
  } catch(e) {
    console.error('데이터 불러오기 실패:', e);
  }
}

// 초기화
setType('shared');
document.getElementById('personalMemberGroup').style.display = 'none';
loadState();
loadApiKey();
updateTripInfo();
</script>
</body>
</html>
