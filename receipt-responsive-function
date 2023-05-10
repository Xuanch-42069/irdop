function view_state_editability(brwForm = _receiptPage) {
    if (is_sample_editable()) {
        // Page State - Edit - Enable
        console.log('--- editable mode detected');
        bapi.helper.runPageActionFromRequest('TvQnOOqAfEiOXW43D7MNag', brwForm);
        // Func - Pre-Cache Edit Pages
        console.log('--- precaching edit pages...');
        bapi.helper.runPageActionFromRequest('xLY7fPSLjEqjpommBBrcSw', brwForm);
    }
    else {
        console.log('--- uneditable mode detected');
        // Page State - Edit - Disable
        bapi.helper.runPageActionFromRequest('EfBBrQoqvE2ehngVXlA4jQ', brwForm);
    }
}

////////////////////////////////////////////////////////////////
function sample_info_view_load(brwForm = _receiptPage) {
    document.getElementById('sample_name_primary_txt').textContent = sample.sample_name_primary || '***';
    document.getElementById('sample_name_secondary_txt').textContent = "/ " + sample.sample_name_secondary || '';
    document.getElementById('sample_mfr_txt').textContent = sample.sample_mfr || '--';
    document.getElementById('sample_qty_txt').textContent = sample.sample_qty || '--';
    document.getElementById('sample_lot_txt').textContent = sample.sample_lot || '--';
    document.getElementById('sample_mfg_txt').textContent = sample.sample_mfg || '--';
    document.getElementById('sample_exp_txt').textContent = sample.sample_exp || '--';
}

function sample_info_view_mode(brwForm = _receiptPage) {
    bapi.helper.runPageActionFromRequest('rYoa3DQLE0iwMPxQHYZJrA', brwForm);
}

function sample_info_edit_mode(brwForm = _receiptPage) {
    bapi.helper.runPageActionFromRequest('zPQVRBgMzkKDAYqTr6FPnA', brwForm); //Page State - Sample Info - Edit Mode
}

function sample_info_edit_load(brwForm = _receiptPage) {
    document.getElementById('sample_name_primary_input').value = sample.sample_name_primary;
    document.getElementById('sample_name_secondary_input').value = sample.sample_name_secondary;
    document.getElementById('sample_mfr_input').value = sample.sample_mfr;
    document.getElementById('sample_qty_input').value = sample.sample_qty;
    document.getElementById('sample_lot_input').value = sample.sample_lot;
    document.getElementById('sample_exp_input').value = sample.sample_exp;
    document.getElementById('sample_mfg_input').value = sample.sample_mfg;
}

function sample_info_validating_inputs(brwForm) {
    let is_valid = true;

    // validating name primary 
    let sample_name_primary = document.getElementById('sample_name_primary_input').value;
    if (sample_name_primary == null || sample_name_primary.length == 0) {
        is_valid = false;
        bapi.helper.runPageActionFromRequest('vBqVqU2bcUG5aHn1hhXBUw', brwForm); // Responsive - Validating inputs - Name Error
    }
    else {
        bapi.helper.runPageActionFromRequest('ArNpIqGDJkWdzhh4yYew4w', brwForm); // Responsive - Validating inputs - Name Correct
    }
    
    // TODO: Ngày sản xuất không được vượt ngày hôm nay
    // TODO: nếu mẫu có hạn sử dụng, không được trước ngày sản xuất 
    return is_valid;
}

async function sample_info_fetch_update() {
    // GET client info
    let sample_info = {
        "sample_name_primary": document.getElementById('sample_name_primary_input').value,
        "sample_name_secondary": document.getElementById('sample_name_secondary_input').value,
        "sample_mfr": document.getElementById('sample_mfr_input').value,
        "sample_qty": document.getElementById('sample_qty_input').value,
        "sample_mfg": document.getElementById('sample_mfg_input').value,
        "sample_exp": document.getElementById('sample_exp_input').value,
        "sample_lot": document.getElementById('sample_lot_input').value,
        "sample_uuid": getSampleUuid()
    }
    
    // Header request
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };
    
    // gửi thông tin mẫu đi xano
    const response = await fetch('https://xn.irdop.org/api:CgA7dN_e/edit/sample_info', 
        {method : "POST", headers: headerApi, body: JSON.stringify(sample_info)});
    
    // Trạng thái phản hồi: Chưa đăng nhập
    if (response.status == 401) {
        window.location.href = '/signin';
    }

    // Lấy data
    const data = await response.json();
    // Trạng thái phản hồi: thành công
    if (response.status == 200 && data != null) {
        console.log(data);
        sample = data;
        sample_info_view_load();
        sample_info_view_mode();
    }

    // phản hồi lỗi chưa xác định (Unexplained response)
    else {
        console.log(data);
        runDialog('Lỗi ' + response.status  +  ': ' + data.message);
    } 
}

////////////////////////////////////////////////////////////////
function client_info_view_load() {
    if (sample.client != null) {
        document.getElementById('client_name_txt').textContent = sample.client.client_name || '--';
        document.getElementById('client_address_txt').textContent = sample.client.client_address || '--';
        document.getElementById('client_legal_id_txt').textContent = sample.client.client_legal_id || '--';
        document.getElementById('client_email_txt').textContent = sample.client.client_email || '--';
        document.getElementById('client_note_txt').textContent = sample.client.client_note || '--';
    }
}

function client_info_open_edit_modal(brwForm = _receiptPage) {
    bapi.helper.runPageActionFromRequest('KyyFDA5NukytI8ujQPjOTg', brwForm); //Func - Edit Client - Open Modal
}

////////////////////////////////////////////////////////////////
function test_info_view_load(refresh_test_list = false, brwForm = _receiptPage) {
    // display tests
    if (refresh_test_list) bapi.helper.getElementNodeFromPage('omebB15XXUiiKIUqrQK3GQ', brwForm).setValue();
    bapi.helper.getElementNodeFromPage('omebB15XXUiiKIUqrQK3GQ', brwForm).setValue(sample.tests);
    // Price Gross
    if (!isNaN(sample.price_gross)) document.getElementById('price_gross_txt').textContent = new Intl.NumberFormat().format(sample.price_gross) || '--';

    // premium price
    if (!isNaN(sample.price_premium)) document.getElementById('price_premium_txt').textContent = new Intl.NumberFormat().format(sample.price_premium) || '--';
    // premium rate
    if (!isNaN(sample.price_premium)) document.getElementById('price_premium_rate_txt').textContent = "+" + new Intl.NumberFormat().format(sample.price_premium_rate*100) +"%";
    else document.getElementById('price_premium_rate_txt').textContent = '--';

    // discount price
    if (!isNaN(sample.price_discount)) document.getElementById('price_discount_txt').textContent = new Intl.NumberFormat().format(sample.price_discount) || '--';
    // discount rate
    if (!isNaN(sample.price_discount)) document.getElementById('price_discount_rate_txt').textContent = "-" + new Intl.NumberFormat().format(sample.price_discount_rate*100) + "%";
    else document.getElementById('price_discount_rate_txt').textContent = '--';

    // tax
    if (!isNaN(sample.price_tax)) document.getElementById('price_tax_txt').textContent = new Intl.NumberFormat().format(sample.price_tax) || '--';
    // tax rate
    if (!isNaN(sample.price_tax_rate)) document.getElementById('price_tax_rate_txt').textContent = '+' + new Intl.NumberFormat().format(sample.price_tax_rate*100) + '%'|| '--';
    // payable
    if (!isNaN(sample.price_payable)) document.getElementById('price_payable_txt').textContent = new Intl.NumberFormat().format(sample.price_payable) || '--';

}

////////////////////////////////////////////////////////////////
function receival_info_view_load(brwForm = _receiptPage){ 
    // sample desc
    document.getElementById('sample_desc_txt').textContent = sample.sample_desc || '--';
    
    // test purpose value
    switch (sample.test_purpose) {
        case '101':
            document.getElementById('test_purpose_txt').textContent = "Công bố";
            break;
    
        case '102':
            document.getElementById('test_purpose_txt').textContent = "Kiểm tra";
            break;
    
        case '103':
            document.getElementById('test_purpose_txt').textContent = "Dự án";
            break;

        default:
            document.getElementById('test_purpose_txt').textContent = sample.test_purpose || "--";
            break;
    }

    // submission method value
    if (sample.submission_method == 101) {
        document.getElementById('submission_method_txt').textContent = "Trực tiếp";
    }
    else if (sample.submission_method == 102) {
        document.getElementById('submission_method_txt').textContent = "Gián tiếp";
    }
    else document.getElementById('submission_method_txt').textContent = sample.submission_method || '--';
    
    // submission method state
    receival_info_submission_state(brwForm);

    // reveived date
    document.getElementById('received_date_txt').textContent = sample.received_date || '--';

    // submitted_by
    document.getElementById('submitted_by_name_txt').textContent = sample.submitted_by_name || '--';
    if (sample.submitted_by_legal_id != '') {
        document.getElementById('submitted_by_name_txt').textContent += " - "  + sample.submitted_by_legal_id;
    }

    // submission courier
    document.getElementById('submission_courier_txt').textContent = sample.submission_courier || '--';

    document.getElementById('recipient_name_txt').textContent = sample.recipient.user_name || '--';

    document.getElementById('days2done_txt').textContent = sample.days2done || '--';

    // result reports
    (function() {
        let publishedCount = getLength(sample.receipts);
        let document_refs = '';
        for (let i = 0; i < publishedCount; i++) {
            document_refs += sample.receipts[i].publish.doc_ref;
            if (i != publishedCount - 1) {
                document_refs += ', ';
            }
        }
        document.getElementById('doc_ref_txt').textContent = document_refs || '--';
    })();

    // language
    (function() {
        let doc_language = 'Tiếng Việt';
        if (sample.doc_secondary_lang) doc_language += " / English";
        document.getElementById('doc_secondary_lang_txt').textContent = doc_language;
    })();
}

function receival_info_view_mode(brwForm = _receiptPage) {
    bapi.helper.runPageActionFromRequest('YS7aZINYhUGlR1d54mmxmg', brwForm); //Page State - Receival Info - View Mode
}

function receival_info_edit_load(brwForm = _receiptPage) {
    // Sample Description
    document.getElementById('sample_desc_input').value = sample.sample_desc;
    
    // Test purpose
    switch (sample.test_purpose) {
        case '101':
            document.querySelector('input[name="new_test_purpose_radio"][value="101"]').checked = true;
            break;
        case '102':
            document.querySelector('input[name="new_test_purpose_radio"][value="102"]').checked = true;
            break;
        case '103':
            document.querySelector('input[name="new_test_purpose_radio"][value="103"]').checked = true;
            break;
        default:
            document.querySelector('input[name="new_test_purpose_radio"][value="104"]').checked = true;
            break;
    }
    receival_info_test_purpose_state(brwForm);

    // Received Date
    document.getElementById('received_date_input').value = sample.received_date;
    
    
    // Submission Method
    if (sample.submission_method == 101) {
        document.querySelector('input[name="new_submission_method_radio"][value="101"]').checked = true;
    }
    else if (sample.submission_method == 102) {
        document.querySelector('input[name="new_submission_method_radio"][value="102"]').checked = true;
    }
    receival_info_submission_state();

    // submitted by
    document.getElementById('submitted_by_name_input').value = sample.submitted_by_name;
    document.getElementById('submitted_by_legal_id_input').value = sample.submitted_by_legal_id;

    // recipient set options
    var selector = document.getElementById('recipient_input').innerHTML = '';
    recipient_list.forEach(obj => {
        const option = document.createElement('option');
        option.value = obj.user_uuid; // Set the value of the option to the uuid
        option.text = obj.user_name; // Set the text of the option to the name
        document.getElementById('recipient_input').add(option); // Add the option to the selector input element
    });

    // set the current selected recipient
    if (sample.recipient.user_uuid != null) {
        document.getElementById('recipient_input').value = sample.recipient.user_uuid;
    }

    // courier
    document.getElementById('submission_courier_input').value = sample.submission_courier;
    
    // days 2 done
    document.getElementById('days2done_input').value = sample.days2done;

    // language
    document.getElementById('doc_secondary_lang_input').checked = sample.doc_secondary_lang;
}

function receival_info_edit_mode(brwForm = _receiptPage) {
    bapi.helper.runPageActionFromRequest('Ikg7hR2vDkGwIQyrnsJDKQ', brwForm); //Page State - Receival Info - Edit Mode
}

function receival_info_test_purpose_state(brwForm = _receiptPage) {
    // get selected test purpose
    let test_purpses = document.querySelectorAll('input[name="new_test_purpose_radio"]');
    let test_purpose;
    for (const item of test_purpses) {
        if (item.checked) {
            test_purpose = item.value;
            break;
        }
    }
    
    // identify test purpose state
    if (test_purpose != 101 && test_purpose != 102 && test_purpose != 103) {

        // mục đích khác
        bapi.helper.runPageActionFromRequest('vkUDNqV7G0AWotOs9f5kag', brwForm); // Elm State - Test Purpose - Select Others
        
        // set purpose to current purpose
        document.getElementById('test_purpose_input').value = '';
    }
    else {
        // mục đích cụ thể:
        bapi.helper.runPageActionFromRequest('t6myQ835FUm2vVAQESzhoQ', brwForm); // Elm State - Test Purpose - Select Specific

        // set purpose to purpose code
        document.getElementById('test_purpose_input').value = test_purpose;
    }

}

function receival_info_submission_state(brwForm = _receiptPage) {
    if (sample.submission_method == 101) {
        bapi.helper.runPageActionFromRequest('kVAcCAShnEiuUBuLrfo2eg', brwForm); // Elm State - Submission method - Direct
    }
    else if (sample.submission_method == 102) {
        bapi.helper.runPageActionFromRequest('9Fm0k7YDs0qpBV0k80SihQ', brwForm); // Elm State - Submission method - InDirect
    }
    else {
        console.log("--- Unfamiliar submission method code detected");
    }
}

function receival_info_submission_edit_state(brwForm = _receiptPage) {
    if (document.querySelector('input[name="new_submission_method_radio"][value="101"]').checked) {
        document.getElementById('submitted_by_name_input').value = sample.submitted_by_name;
        document.getElementById('submitted_by_legal_id_input').value = sample.submitted_by_legal_id;
        bapi.helper.runPageActionFromRequest('kVAcCAShnEiuUBuLrfo2eg', brwForm); // Elm State - Submission method - Direct
        document.getElementById('submission_courier_input').value = '';
    }
    else if (document.querySelector('input[name="new_submission_method_radio"][value="102"]').checked) {
        document.getElementById('submission_courier_input').value = sample.submission_courier;
        bapi.helper.runPageActionFromRequest('9Fm0k7YDs0qpBV0k80SihQ', brwForm); // Elm State - Submission method - InDirect
        document.getElementById('submitted_by_name_input').value = '';
        document.getElementById('submitted_by_legal_id_input').value = '';
    }
}

function receival_info_validating_inputs(brwForm = _receiptPage) {
    let isAllValid = true;

    // soát mục đích kiểm nghiệm
    let test_purposes = document.getElementById('test_purpose_input').value;
    if (test_purposes == null || test_purposes == '') {
        console.log('Invalid test purpose');
        // Validating - Test Purpose - Invalid
        bapi.helper.runPageActionFromRequest('CwpWxSywgU6SvTXUjvOeBg', brwForm);
        isAllValid = false;
    }
    // mục đích kiểm nghiêm hợp lệ
    else {
        // Validating - Test Purpose - Valid
        bapi.helper.runPageActionFromRequest('LhAa0KAld0G7agkDOkAtzQ', brwForm);
    }

    // Phương thức gửi mẫu để trống
    if (document.querySelector('input[name="new_submission_method_radio"][value="101"]').checked == false
        && document.querySelector('input[name="new_submission_method_radio"][value="102"]').checked == false) {
        // Validating - Submission Method - Invalid 
        bapi.helper.runPageActionFromRequest('ee7yAGIHAUWMHAAyGXEalQ', brwForm);
        isAllValid = false;
    }
    // Phương thức gửi mẫu hợp lệ
    else {
        // Validating - Submission Method - Valid
        bapi.helper.runPageActionFromRequest('9lW4bfq4H0atBNBldAWaGA', brwForm);
    }

    return isAllValid;
}

async function receival_info_fetch_update() {
    // read submission_method
    let submission_methods = document.querySelectorAll('input[name="new_submission_method_radio"]');
    let submission_method;
    for (const item of submission_methods) {
        if (item.checked == true) {
            submission_method = item.value;
            break;
        }
    }

    let receival_info = {
        "sample_uuid": getSampleUuid(),
        "sample_desc": document.getElementById('sample_desc_input').value,
        "test_purpose": document.getElementById('test_purpose_input').value,
        "received_date": document.getElementById('received_date_input').value,
        "recipient_uuid": document.getElementById('recipient_input').value,
        "submission_method": submission_method,
        "submitted_by_name": document.getElementById('submitted_by_name_input').value,
        "submitted_by_legal_id": document.getElementById('submitted_by_legal_id_input').value,
        "submission_courier": document.getElementById('submission_courier_input').value,
        "days2done": document.getElementById('days2done_input').value,
        "doc_secondary_lang": document.getElementById('doc_secondary_lang_input').checked
      }
    
    // Header request
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };
    
    // gửi thông tin mẫu đi xano
    const response = await fetch('https://xn.irdop.org/api:CgA7dN_e/edit/receival_info', 
        {method : "POST", headers: headerApi, body: JSON.stringify(receival_info)});
    
    // Trạng thái phản hồi: Chưa đăng nhập
    if (response.status == 401) {
        window.location.href = '/signin';
    }

    // Lấy data
    const data = await response.json();
    // Trạng thái phản hồi: thành công
    if (response.status == 200 && data != null) {
        console.log(data);
        sample = data;
        toggle_mode_receival_info(1);   
    }

    // phản hồi lỗi chưa xác định (Unexplained response)
    else {
        console.log(data);
        runDialog('Lỗi ' + response.status  +  ': ' + data.message);
    }
}

////////////////////////////////////////////////////////////////
function invoice_info_view_load() {
    document.getElementById('invoice_name_txt').textContent = sample.invoice_name || '--';
    document.getElementById('invoice_address_txt').textContent = sample.invoice_address || '--';
    document.getElementById('invoice_tax_code_txt').textContent = sample.invoice_tax_code || '--';
    document.getElementById('invoice_email_txt').textContent = sample.invoice_email || '--';
}

function invoice_info_view_mode(brwForm = _receiptPage) {
    bapi.helper.runPageActionFromRequest('FYZu459KEUaBTFmUsaZmlw', brwForm); // Page State - Invoice View mode
}

function invoice_info_edit_load() {
    document.getElementById('invoice_name_input').value = sample.invoice_name;
    document.getElementById('invoice_email_input').value = sample.invoice_email;
    document.getElementById('invoice_address_input').value = sample.invoice_address;
    document.getElementById('invoice_tax_code_input').value = sample.invoice_tax_code;
}

function invoice_info_edit_mode(brwForm = _receiptPage) {
    bapi.helper.runPageActionFromRequest('iGFWlfu8rU2B9VFjXCbh1g', brwForm); // Page State - Invoice Edit mode
}


////////////////////////////////////////////////////////////////
async function client_search_fetch(brwForm) {
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };
    const xano_input =
    {
        "query": document.getElementById('new_client_search_input').value
    };

    try {
        const response = await fetch('https://xn.irdop.org/api:CgA7dN_e/client/search', 
            {method : "POST", headers: headerApi, body: JSON.stringify(xano_input)});
        
        // response = 401: Chưa đăng nhập
        if (response.status == 401) window.location.href ='/signin';
    
        // Get requested data
        const data = await response.json();
        
        // request success
        if (response.status == 200 && data != null) {
            // set result to repeated list element
            bapi.helper.getElementNodeFromPage('njVsBaSR9kaTYf73URlGAg', brwForm).setValue(data);
            bapi.helper.runPageActionFromRequest('wvJ4Q5HAekazkBIIrRN6hw', brwForm); // Page State - Display Search Client Results
        }
    
        // request failed
        else {
            runDialog('Lỗi ' + response.status + ': ' + data.message);
            console.log('--- Fetch Search Client failed: ');
            console.log(data);
        }

    } catch (error) {
        console.log('--- Catch error fetching search query: ' + error);
        runDialog('Lỗi khi gửi yêu cầu. Vui lòng thử lại sau');
    }
}

async function client_fetch_selected_client(client_uuid) {
    if (client_uuid == null) {
        return;
    }
    
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };
    const xano_input = {
        "sample_uuid": getSampleUuid(),
        "client_uuid": client_uuid
    };

    try {
        const response = await fetch('https://xn.irdop.org/api:CgA7dN_e/client/pick_a_client_to_receipt', 
            {method : "POST", headers: headerApi, body: JSON.stringify(xano_input)});
        
        // response = 401: Chưa đăng nhập
        if (response.status == 401) window.location.href ='/signin';
    
        // Get requested data
        const data = await response.json();
        
        // request success
        if (response.status == 200 && data != null) {
            sample = data;
            client_info_view_load();
            close_modal();
        }
    
        // request failed
        else {
            runDialog('Lỗi ' + response.status + ': ' + data.message);
            console.log('--- Pick client request fail. client_uuid: ' + client_uuid);
            console.log(data);
        }

    } catch (error) {
        console.log('--- Catch error fetching client picking: ' + error);
        runDialog('Lỗi khi gửi yêu cầu. Vui lòng thử lại sau');
    }
}

async function client_fetch_new_client(brwForm) {
    // GET client info
    let new_client = {
        "client_name" : document.getElementById('new_client_name_input').value,
        "client_address" : document.getElementById('new_client_address_input').value,
        "client_legal_id" : document.getElementById('new_client_legal_id_input').value,
        "client_email" : document.getElementById('new_client_email_input').value,
        "contact_person_phone": document.getElementById('new_contact_person_phone_input').value,
        "client_note": document.getElementById('new_client_note_input').value,
        "sample_uuid": getSampleUuid()
    };
    
    // Header request
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };
    
    // gửi thông tin khách hàng đi xano
    const response = await fetch('https://xn.irdop.org/api:CgA7dN_e/client/create_new', 
        {method : "POST", headers: headerApi, body: JSON.stringify(new_client)});
    
    // Trạng thái phản hồi: Chưa đăng nhập
    if (response.status == 401) {
        window.location.href = '/signin';
    }

    const data = await response.json();
    // Trạng thái phản hồi: OK
    if (response.status == 200 && data != null) {
        console.log(data);
        sample = data;
        client_info_view_load();
        close_modal();
    }

    // phản hồi lỗi chưa xác định (Unexplained response)
    else {
        console.log(data);
        runDialog('Lỗi ' + response.status  +  ': ' + data.message);
    }
}

////////////////////////////////////////////////////////////////
async function test_name_primary_fetch(_arrayIndex, test_uuid, brwForm) {
    // if nothing changed , do not fetch
    if (bapi.helper.getElementNodeFromPage('PtoyirId0USjWqLKCrvSew', brwForm).getValue() == sample.tests[_testIndex].test_name_primary) {
        return;
    }

    let xano_input = {
        "sample_test_uuid": test_uuid,
        "test_name_primary": bapi.helper.getElementNodeFromPage('PtoyirId0USjWqLKCrvSew', brwForm).getValue()
    };
    
    // Header request
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };
    
    // gửi thông tin khách hàng đi xano
    const response = await fetch('https://xn.irdop.org/api:CgA7dN_e/edit/test_name', 
        {method : "POST", headers: headerApi, body: JSON.stringify(xano_input)});
    
    // Trạng thái phản hồi: Chưa đăng nhập
    if (response.status == 401) {
        window.location.href = '/signin';
    }

    const data = await response.json();
    // Trạng thái phản hồi: OK
    if (response.status == 200 && data != null) {
        console.log(data);
        sample.tests[_arrayIndex] = data;
        bapi.helper.getElementNodeFromPage('Q3qil17rakWg7UJLqSQgmQ', brwForm).setValue(data.test_name_primary);
    }

    // phản hồi lỗi chưa xác định (Unexplained response)
    else {
        console.log(data);
        runDialog('Lỗi ' + response.status  +  ': ' + data.message);
    }
}

async function test_protocol_search_fetch(brwForm) {
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };
    const xano_input =
    {
        "query": document.getElementById('protocol_search_query_input').value
    };

    try {
        const response = await fetch('https://xn.irdop.org/api:_gbH5ku_/protocol/search', 
            {method : "POST", headers: headerApi, body: JSON.stringify(xano_input)});
        
        // response = 401: Chưa đăng nhập
        if (response.status == 401) window.location.href ='/signin';
    
        // Get requested data
        const data = await response.json();
        
        // request success
        if (response.status == 200) {
            // set result to repeated list element
            if (data.length > 0) { 
                bapi.helper.getElementNodeFromPage('wG1jYBCLIECCYv6UAenb1w', brwForm).setValue(data); //
                bapi.helper.runPageActionFromRequest('T9kJ2sYas0BAoihWHLvGrw', brwForm); // Page State - Search Prompt
            }
            else {
                bapi.helper.runPageActionFromRequest('TEUSBGvnTUCsbBgfrB5fMQ', brwForm); // Page State - No Result
            }
        }
    
        // request failed
        else {
            runDialog('Lỗi ' + response.status + ': ' + data);
            console.log('--- Fetch Search Client failed: ');
            console.log(data);
        }

    } catch (error) {
        console.log('--- Catch error fetching search query: ' + error);
        runDialog('Lỗi khi gửi yêu cầu. Vui lòng thử lại sau');
    }
}

async function test_protocol_pick_fetch(_testIndex, protocol_uuid, brwForm) {
    let xano_input = {
        "sample_test_uuid": sample.tests[_testIndex].sample_test_uuid,
        "protocol_uuid": protocol_uuid
    };

    // Header request
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };

    // gửi thông tin khách hàng đi xano
    const response = await fetch('https://xn.irdop.org/api:CgA7dN_e/edit/test_protocol', 
        {method : "POST", headers: headerApi, body: JSON.stringify(xano_input)});

    // Trạng thái phản hồi: Chưa đăng nhập
    if (response.status == 401) {
        window.location.href = '/signin';
    }

    const data = await response.json();
    // Trạng thái phản hồi: OK
    if (response.status == 200) {
        console.log(data);
        if (data!= null) {
            sample.tests[_testIndex] = data;
            test_info_view_load(true);
            close_modal();
        }
        else {
            runDialog('Lỗi khi gửi yêu cầu. Vui lòng thử lại sau');
            console.log(data);
            close_modal();
        }
    }

    // phản hồi lỗi chưa xác định (Unexplained response)
    else {
        console.log(data);
        runDialog('Lỗi ' + response.status  +  ': ' + data.message);
    }
}

async function test_std_ref_fetch(_testIndex, brwForm) {
    // if nothing changed , do not fetch
    if (bapi.helper.getElementNodeFromPage('YN9prfD2dkiYhawKRBNZzQ', brwForm).getValue() == sample.tests[_testIndex].std_ref) {
        return;
    }
    
    let xano_input = {
        "test_uuid": sample.tests[_testIndex].sample_test_uuid,
        "std_ref": bapi.helper.getElementNodeFromPage('YN9prfD2dkiYhawKRBNZzQ', brwForm).getValue()
    };
    
    // Header request
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };
    
    // gửi thông tin đi xano
    const response = await fetch('https://xn.irdop.org/api:CgA7dN_e/edit/test_std_ref', 
        {method : "POST", headers: headerApi, body: JSON.stringify(xano_input)});
    
    // Trạng thái phản hồi: Chưa đăng nhập
    if (response.status == 401) {
        window.location.href = '/signin';
    }

    const data = await response.json();
    // Trạng thái phản hồi: OK
    if (response.status == 200 && data != null) {
        console.log(data);
        sample.tests[_testIndex] = data;
        bapi.helper.getElementNodeFromPage('2l8ovOACpUGdxKciCKbNhQ', brwForm).setValue(data.std_ref);
    }

    // phản hồi lỗi chưa xác định (Unexplained response)
    else {
        console.log(data);
        runDialog('Lỗi ' + response.status  +  ': ' + data.message);
    }
}

async function test_fee_fetch(_testIndex, brwForm) {
    let new_fee = bapi.helper.getElementNodeFromPage('74qOyWVT10ucKtTSbigj1Q', brwForm).getValue();
    // if nothing changed , do not fetch
    if (new_fee == sample.tests[_testIndex].fee) return;

    // if fee < 0 or NaN
    if (isNaN(new_fee) || new_fee < 0)  return;

    let xano_input = {
        "sample_test_uuid": sample.tests[_testIndex].sample_test_uuid,
        "fee": new_fee
    };
    
    // Header request
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };
    
    // gửi thông tin đi xano
    const response = await fetch('https://xn.irdop.org/api:CgA7dN_e/edit/test_fee', 
        {method : "POST", headers: headerApi, body: JSON.stringify(xano_input)});
    
    // Trạng thái phản hồi: Chưa đăng nhập
    if (response.status == 401) {
        window.location.href = '/signin';
    }

    const data = await response.json();
    // Trạng thái phản hồi: OK
    if (response.status == 200 && data != null) {
        console.log(data);
        sample = data;
        bapi.helper.getElementNodeFromPage('gie35rh6a0AdkZmaYPX1gg', brwForm).setValue(new Intl.NumberFormat().format(data.tests[_testIndex].fee) || ''); // set fee value to txt
        test_info_view_load(false);
    }

    // phản hồi lỗi chưa xác định (Unexplained response)
    else {
        console.log(data);
        runDialog('Lỗi ' + response.status  +  ': ' + data.message);
    }
}

async function test_prices_fetch (premium_rate, discount_rate, tax_rate, brwForm) {
    let xano_input = {
        "sample_uuid": getSampleUuid(),
        "price_premium_rate": premium_rate,
        "price_discount_rate": discount_rate,
        "price_tax_rate": tax_rate
    };
    
    // Header request
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };
    
    // gửi thông tin đi xano
    const response = await fetch('https://xn.irdop.org/api:CgA7dN_e/edit/test_prices', 
        {method : "POST", headers: headerApi, body: JSON.stringify(xano_input)});
    
    // Trạng thái phản hồi: Chưa đăng nhập
    if (response.status == 401) {
        window.location.href = '/signin';
    }

    const data = await response.json();
    // Trạng thái phản hồi: OK
    if (response.status == 200 && data != null) {
        console.log(data);
        sample = data;
        test_info_view_load();
    }

    // phản hồi lỗi chưa xác định (Unexplained response)
    else {
        console.log(data);
        runDialog('Lỗi ' + response.status  +  ': ' + data.message);
    }
}

async function test_new_test(brwForm = _receiptPage){
    if (document.getElementById("new_test_primary_name_input").value == '') return;
    let xano_input = {
        "sample_uuid": getSampleUuid(),
        "test_name_primary" : document.getElementById("new_test_primary_name_input").value
    };
    
    // Header request
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };
    
    // gửi thông tin đi xano
    const response = await fetch('https://xn.irdop.org/api:CgA7dN_e/edit/test_new', 
        {method : "POST", headers: headerApi, body: JSON.stringify(xano_input)});
    
    // Trạng thái phản hồi: Chưa đăng nhập
    if (response.status == 401) {
        window.location.href = '/signin';
    }

    const data = await response.json();
    // Trạng thái phản hồi: OK
    if (response.status == 200 && data != null) {
        console.log(data);
        sample = data;
        bapi.helper.runPageActionFromRequest('lY7CgDoTrkahsPyBzTwdlg', brwForm);
        test_info_view_load();
    }

    // phản hồi lỗi chưa xác định (Unexplained response)
    else {
        console.log(data);
        runDialog('Lỗi ' + response.status  +  ': ' + data.message);
    }
    
}

async function test_delete_fetch(sample_test_uuid, brwForm) {
    if (sample_test_uuid == null) {
        console.log('--- Invalid code: Sample test uuid is null');
        return;
    }

    let xano_input = {
        "sample_test_uuid": sample_test_uuid
    };
    
    // Header request
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };
    
    // gửi thông tin đi xano
    const response = await fetch('https://xn.irdop.org/api:CgA7dN_e/edit/test_delete', 
        {method : "POST", headers: headerApi, body: JSON.stringify(xano_input)});
    
    // Trạng thái phản hồi: Chưa đăng nhập
    if (response.status == 401) {
        window.location.href = '/signin';
    }

    const data = await response.json();
    // Trạng thái phản hồi: OK
    if (response.status == 200 && data != null) {
        console.log(data);
        sample = data;
        test_info_view_load(true);
    }

    // phản hồi lỗi chưa xác định (Unexplained response)
    else {
        console.log(data);
        runDialog('Lỗi ' + response.status  +  ': ' + data.message);
    }
}

////////////////////////////////////////////////////////////////

async function invoice_info_fetch(){
    let invoice_info = {
        "sample_uuid" : getSampleUuid(),
        "invoice_name" : document.getElementById('invoice_name_input').value,
        "invoice_address" : document.getElementById('invoice_address_input').value,
        "invoice_tax_code": document.getElementById('invoice_tax_code_input').value,
        "invoice_email": document.getElementById('invoice_email_input').value
    }
    
    // Header request
    const headerApi = {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        'Authorization': 'Bearer ' + getUserAuth()
    };
    
    // gửi thông tin mẫu đi xano
    const response = await fetch('https://xn.irdop.org/api:CgA7dN_e/edit/sample_invoice', 
        {method : "POST", headers: headerApi, body: JSON.stringify(invoice_info)});
    
    // Trạng thái phản hồi: Chưa đăng nhập
    if (response.status == 401) {
        window.location.href = '/signin';
    }

    // Lấy data
    const data = await response.json();
    // Trạng thái phản hồi: thành công
    if (response.status == 200 && data != null) {
        console.log(data);
        sample = data;
        invoice_info_view_load();
        invoice_info_view_mode();
    }

    // phản hồi lỗi chưa xác định (Unexplained response)
    else {
        console.log(data);
        runDialog('Lỗi cập nhật invoice: ' + response.status  +  ': ' + data.message);
    }
}

