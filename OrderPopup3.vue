<template>
    
    <!-- :min-width="200" :min-height="200" :resizable="true" -->
    <modal name="order-popup3" height="790px"  width="1085px" :draggable="'.mobileclick'" :adaptive="true" :clickToClose="false"
        @before-open="beforeOpen" id="order-popup">
        <!-- wrap -->
        <div class="order_popup_content">
            <!-- title -->
            <div class="order_header">
                <div style="color: #4a4a4a; margin-top:5px; margin-left:5px;">
                    상품 접수 및 결제하기
                    <!-- <div v-if="companyName=='Y'" style="margin-left:40px;" class="box">
                        <img :src="require(`@/assets/${COMKEY}.png`)" style="height:15px;margin-right:10px;" alt="banner">
                        <span class="box-label"> {{ GMNM }}</span>
                    </div> -->
                
                
                </div>
          <!--      <span v-if="companyName == 'Y'" style="position: absolute; left: 230px; top:12px; font-size:25px;font-weight:bold;color:red;">( {{GMNM}} )</span>-->
                <div>
                    <a class="subtitle" style='float: right' @click="checkUser">
                        <img src="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTYiIGhlaWdodD0iMTYiIHZpZXdCb3g9IjAgMCAxNiAxNiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4NCjxwYXRoIGQ9Ik0xNC44NDkyIDAuMDcxMjg5MUwxNS41NTYzIDAuNzc4Mzk2TDAuNzA3MTA3IDE1LjYyNzZMMCAxNC45MjA1TDE0Ljg0OTIgMC4wNzEyODkxWiIgZmlsbD0iIzYzNjc2RSIvPg0KPHBhdGggZD0iTTMuMDkwODNlLTA4IDAuNzc4Mzk2TDAuNzA3MTA2IDAuMDcxMjg5N0wxNS41NTYzIDE0LjkyMDVMMTQuODQ5MiAxNS42Mjc2TDMuMDkwODNlLTA4IDAuNzc4Mzk2WiIgZmlsbD0iIzYzNjc2RSIvPg0KPC9zdmc+DQo="></a>
                </div> 
            </div>
            <!-- 상단 -->
            <div style="display:flex; justify-content: left;">
             <!-- 고객검색 라인 -->
             <div style="align-items: center; margin-top:60px; border-style: none; margin-right:15px;" v-bind:class="{add_opacity: !custView}" >
                            <span class="set_bold" 
                            style="color: #006bc9 !important;  padding: 3px; border-radius: 5px; margin: 2px 5px 0px 3px; ">
                            고객검색</span>
                            <select class="custom_select" style="margin-left: 5px; text-align:center;" v-model="srchField0" :disabled="!custView">
                                <option value="0401">일반개인</option>
                                <option value="0402">사업자</option>
                                <option value="ALL">전체</option>
                            </select>
                            <select class="custom_select" style="margin-left: 2px; text-align:center;" v-model="srchField" :disabled="!custView">
                                <option v-for="(item, idx) in srchOps" v-bind:key="idx" :value="item.data">{{ item.label }}</option>
                            </select>

                            <input type='text' class="custom_input" style="margin-right: 5px; height: 25px; width:120px;" 
                                :disabled="!custView" v-model="srchWord" @keyup.enter="RSELECT='N'; sessionChk('sendData1')" @input="setTextPattern">
                            
                            <span v-if="newOrder=='N'" >
                                <!--<a @click="RSELECT='N'; sessionChk('sendData1')" 
                                    :disabled="!custView" style="margin-right: 5px;"><img src="../../../assets/pop_order/search_small.png"
                                        alt="search_small"></a>-->
                                <button @click="RSELECT='N'; sessionChk('sendData1')" 
                                    :disabled="!custView" class="btn_search">검색</button>
                                <button v-if="newOverlap == 'Y'" class="db_new" @click="RSELECT ='NN'; sessionChk('sendData1');" v-bind:class="{before_opacity: !BCUM01}">중복신규</button>
                                <button v-if="newOrder == 'Y'" class="db_new" @click="RSELECT ='NN'; sessionChk('sendData1');">신규</button>
                            </span>
                                
                        </div>
                        <div style="">
                            <div class="custom_info">
                            <span class="label_color"
                                style="margin-right: 5px; padding:3px 0 3px 3px;">고객명</span>
                            <input class="custom_input" type='text' style="width: 90px; margin-right: 10px;" v-model="custnm" ref="custnm" :disabled="!srchOk">
                           
                            <span class="label_color" style="margin-right: 5px;">전화번호</span>
                            <input class="custom_input" type='text' style="width: 100px; margin-right: 10px;"
                                v-model="custtel" ref="custtel" :disabled="!custtelenabled" @keypress="onlyNumber">
                                </div>
                                <div class="custom_info">
                            <span class="label_color" style="margin-right: 5px; padding-left:3px;">사업자번호</span>
                            <input class="custom_input" type='text' style="width: 150px; margin-right: 10px;" readonly v-model="cfcompno" @keypress="onlyNumber">
                            
                            <span class="label_color" style="margin-right: 5px;">이메일</span>
                            <input class="custom_input" type='text' style="margin-right: 10px; width:160px" v-model="custemail" :disabled="!srchOk">
                            </div>
                            <div class="custom_info">
                            <span class="label_color" style=" margin-right: 5px; padding:3px 0 3px 3px;">주&nbsp;&nbsp;&nbsp;&nbsp;소</span>
                            <input class="custom_input" type='text' style=" margin-right:10px; width:300px" v-model="addr">
                            <button @click="viewType='customer'" v-bind:class="{before_opacity: !BCUM01}"
                            :disabled="!BCUM01" class="db_new" style=" width:100px;">고객상세내역</button>
                        </div>
                        </div>
                        </div>

            <!-- 내용 부분 -->
            <div class="order_content_wrap" style="display:flex">

              
                <!-- 왼쪽 -->
                <div style="float:left;" >
                                        <!-- 왼쪽(꼬리말X) -->
                                        <!-- <div class="" style="margin-bottom: 5px;"> -->
                                    
                                    <!-- <div class="flex_row"> -->
                    <!-- 고객상세내역팝업 위치 -->
                    <div  style="width:635px; margin-right: 10px;" v-if="viewType=='grid'">
                        
                        <!-- 왼쪽 중단 -->
                        <div style="border-radius: 5px; margin-bottom: 5px; padding: 5px; width:635px; border: 1px solid #eeeeee;">
                            <!-- 상품 위 -->
                            <div class="flex_row" style="padding: 5px;font-size: 13px; ">
                                <!-- 검색창 -->
                                <div style="border:1px solid #EEEEEE; border-radius: 3px; display:flex; padding-left: 5px; ">
                                    <img src="../../../assets/popup_ver1.1.70/Vector.png" style="margin: auto">
                                    <input class="input_date" type="text" v-model="searchStr" @change="findItem" placeholder="상품명"
                                        style="height: 24px !important; width: 190px; margin-right: 5px; padding:0px 10px!important; margin-top: 2px;  border:0px !important;" v-bind:class="{add_opacity: !itemView}">
                                        <a v-bind:class="{add_opacity: !itemView}" style="margin-left: 5px;"><button class="btn_search" style="border-radius:3px;">검색</button></a>
                               </div>
                                <span style="" class="input_label">
                                    <label v-bind:class="{add_opacity: !itemView}" class="inputTypeRadio" >
                                        <input type="radio" name="inputTypeRadio" style="" value="메뉴그룹사용"
                                            v-model="inputTypeRadio" :disabled="!itemView"> 메뉴그룹사용</label>
                                    <label v-bind:class="{add_opacity: !itemView}" class="inputTypeRadio" onclick="document.getElementById('radio_focus').focus()">
                                        <input id="radio_focus" type="radio" name="inputTypeRadio" style="" value="상품직접입력"
                                            v-model="inputTypeRadio" :disabled="!itemView"> 상품직접입력</label>
                                </span>
                            </div>
                            <!-- 대분류+상품+상품직접입력 -->
                            <div style="display:flex; flex:0;">
                                <!-- 대분류+상품 -->
                                <div style="width:625px;" v-bind:class="{add_opacity: !itemView}" >
                                    <!-- 대분류 선택 -->
                                    <div class="" style="padding: 5px; display:flex; max-width:625px; justify-content: space-between; height: 45px; ">
                                        <!-- <button style="width: 35px; margin-right: 5px; background: white !important;"><i
                                                class="fas fa-bars"></i></button> -->
                                        <button class="group_btn_off" style="width: 35px;  border: 1px solid rgba(57, 60, 63, 0.15); background:#ffffff;"
                                            @click="groupNavi('prev')"><i class="fas fa-angle-left" style=" color: #B0AFAF;"></i></button>
                                        <div style="" class="flex_row">
                                            <button @click="clickGroupBtn(item.title)" v-for="item in groupBtn"
                                                v-bind:key="item.title"
                                                :class="{group_btn_on: item.clicked, group_btn_off: !item.clicked}"
                                                style="width:90px; height: 35px; margin: 0 5px;">{{ item.title }}</button>
                                        </div>
                                        <button class="group_btn_off" style="width: 35px;  border: 1px solid rgba(57, 60, 63, 0.15); background:#ffffff;" @click="groupNavi('next')" ><i
                                                class="fas fa-angle-right" style=" color: #B0AFAF;"></i></button>
                                    </div>

                                    <!-- 상품 그리드 -->
                                    <div class="" style="height: 300px; overflow-y: scroll; max-width:625px; padding:5px; ">
                                        <div v-for="(item, index) in filteredList" v-bind:key="item.v_cod"
                                            :class="{grid_item_on: item.clicked, grid_item_off: !item.clicked, grid_margin3: selfView, grid_item2: selfView, grid_item: !selfView}"
                                            @click="gridClick(index)" style="">
                                            <span class="grid_group_title">{{ item.rt_reg1 }}</span>
                                            <span class="grid_title">{{ item.v_nam }}</span>
                                            <p class="grid_price_title">{{ makeComma(item.rt_pay) }} 원</p>
                                        </div>
                                    </div>
                                <!-- 대분류+상품 -->
                                </div>
                                    <!-- 상품 직접 입력 -->
                                    <div v-if="selfView" class="order_content_right"
                                        style=" max-width:180px; background: white;  border-radius:5px; margin: 15px 15px 0px 20px;">
                                        <div  
                                        style="font-size: 14px; border-bottom: 1px solid var(--color-tp-text-black); color: var(--color-tp-text-black); border-top-left-radius:5px;
                                        border-top-right-radius:5px; margin-bottom:50px; padding-bottom:10px; font-weight:bolder;">상품직접입력</div>
                                        <div style="padding-right:10px;">
                                            <div style="margin-bottom: 15px; ">
                                                <span class="self_label" style="padding-top: 2px;">상품명<br><span
                                                        style='color: var(--color-tp-main-blue); font-size: 12px; padding-right:2px;'>(필수)</span></span>
                                                <input type="text" class="input_date"
                                                    style="width: 125px; height: 30px; border: 1px solid #E0E0E0; font-size: 17px; padding-left: 5px;"
                                                    v-model="selfItemName">
                                            </div>
                                            <div class="" style="margin-bottom: 15px;">
                                                <span class="self_label" style="padding-top: 8px; text-align: center;">수량</span>
                                                <input type="text" class="input_date"
                                                    style="width: 125px; height: 30px; border: 1px solid #E0E0E0; font-size: 17px; text-align: right;padding-right: 5px;"
                                                    v-model="selfCount_span" @keypress="onlyNumber">
                                            </div>
                                            <div class="" style="margin-bottom: 15px;">
                                                <span class="self_label" style="padding-top: 8px; text-align: center;">단가</span>
                                                <input type="text" class="input_date"
                                                    style="width: 125px; height: 30px; border: 1px solid #E0E0E0; font-size: 17px; text-align: right;padding-right: 5px;"
                                                    v-model="selfUnitPrice_span" @keypress="onlyNumber">
                                            </div>
                                            <div class="flex_row" style="margin-bottom: 25px;">
                                                <span class="self_label" style="padding-top: 8px; text-align: center;">금액</span>
                                                <input type="text" class="input_date"
                                                    style="width: 125px; height: 30px; border: 1px solid #E0E0E0; font-size: 17px; text-align: right;padding-right: 5px;"
                                                    v-model="selfPrice_span" readonly>
                                            </div>  
                                            <button style="float: right; margin-right: 3px; background: #ffffff; color: #006BC9; border: 1px solid rgb(0, 107, 201); padding-bottom:2px;  width: 60px; height: 28px;border-radius: 5px; "
                                                    @click="selfAdd()">추가
                                            </button>
                                        </div>
                                    <!-- 상품직접입력 -->
                                    </div>

                                </div>
                            <!-- 왼쪽 중단 -->
                            </div>
                                <!-- <div style=" border-radius: 3px;  width:670px; " class="flex_row"> -->
                            <!-- 결제+영수증 -->
                            <div style="padding: 5px; padding-bottom: 0px; display:flex; height:170px;">
                                <!-- 결제부분 -->
                                <div style="float:left; width:335px;">
                                        <!-- <div class="" style="height:40px;font-weight: 400; font-size: 14px;padding-left: 10px; line-height: 40px; color: #FFFFFF; width: 100px;  background-color: #006bc9; border-top-left-radius: 5px;">결제수단선택
                                        </div> -->
                                
                                    <!-- 결제수단선택 -->                           
                                    <div class="" style="display:flex; width:335px;">
                                        <label style="" @click="strcfrmtype = '0103'" class="flex_row" v-bind:class="{add_opacity: !payCheckView}">
                                            <!-- <input type="checkbox"v-if="strcfrmtype == '0103'" alt="check"> -->
                                            <!-- <input type="checkbox" v-if="strcfrmtype != '0103'" alt="check"> -->
                                            <!-- <input type="radio" :disabled="!payCheckView" v-model="inputTypeRadio2" name="inputTypeRadio2" value="카드결제" > -->
                                            <span class="card_pay"  :disabled="!payCheckView" v-model="inputTypeRadio2" name="inputTypeRadio2" value="카드결제" 
                                            style="border-bottom: 1px solid #f1f1f1; color: #acacac; padding-bottom: 10px;" :class="{card_pay_on:strcfrmtype == '0103'}">
                                            카드결제</span>
                                        </label>
                                        <p style="    border-bottom: 1px solid #f1f1f1;   color: #acacac; width: 40px;"></p>
                                        <label style="" @click="ButtonCheck('0104')" class="flex_row" v-bind:class="{add_opacity: !payCheckView}">
                                        <!-- <input type="checkbox" v-if="strcfrmtype == '0104'" alt="check"> -->
                                        <!-- <input type="checkbox"  v-if="strcfrmtype != '0104'" alt="check"> -->
                                        <!-- <input type="radio" :disabled="!payCheckView" v-model="inputTypeRadio2" name="inputTypeRadio2" value="현금결제"> -->


                                        <p class="cash_pay"  :disabled="!payCheckView" v-model="inputTypeRadio2" name="inputTypeRadio2" value="현금결제" 
                                        style="padding-bottom: 10px; border-bottom: 1px solid #f1f1f1; color: #acacac;" :class="{cash_pay_on:strcfrmtype == '0104'}">
                                        현금결제</p>
                                        </label>
                                        <p style="    border-bottom: 1px solid #f1f1f1; color: #acacac; width: 40px;"></p>
                                        <label style="" @click="strcfrmtype = '0102'" class="flex_row" v-bind:class="{add_opacity: !payCheckView}">
                                        <span class="self_pay"  :disabled="!payCheckView" v-model="inputTypeRadio2" name="inputTypeRadio2" value="고객직접결제" 
                                        style="   border-bottom: 1px solid #f1f1f1; color: #acacac; padding-bottom: 10px;" :class="{self_pay_on:strcfrmtype == '0102'}">
                                            고객직접결제</span>
                                        </label>
                                        <p style="   border-bottom: 1px solid  #f1f1f1; color: #acacac;  width:87px;"></p>
                                    </div>
                                        
                                    <!-- 고객직접결제 -->
                                    <div class="order_content_left"style="float:left; margin:0px; margin-top:10px;" v-if="strcfrmtype == '0102'">
                                        <!-- <span
                                            style="display: inline-block; font-size: 14px; width:100%; margin-bottom: 10px;">
                                            고객직접결제
                                        </span> -->
                                        <!-- <div style="padding: 5px 20px; " > -->
                                        <div  style=" padding: 3px;  margin: 7px 5px 10px 3px;">
                                            <label style="display:flex; margin-right:10px; margin-bottom:10px; float:left;">
                                            <input type="radio" style="margin-top:1.5px; margin-right: 10px; margin-left: 5px;" checked> 문자전송</label>
                                        </div>
                                        <div style="display:inline-block;  margin-bottom:5px;">
                                            <input class="custom_input" style="width: 200px; font-weight:normal !important;"
                                             type="text" placeholder="전화번호" v-model="urlTel"  ref="urlTel" @keypress="onlyNumber">
                                            <button class="btn_resend" style=""
                                            v-bind:class="{add_opacity: !payResendView || strcfrmtype=='0104'}" @click="sessionChk('ftranSend6')" >
                                            결제창재전송</button>
                                        </div>
                                    
                                        <div style="padding: 5px; margin-top:3px;">
                                            <span class="card_label gray_bold">결제상태</span>
                                            <span>{{ ordermsg0 }}<br v-if="ordermsg != ''">{{ ordermsg }}</span>
                                        </div>
                                        <div style="padding: 5px;">
                                            <span class="card_label gray_bold">취소사유</span>
                                            <input v-if="partial" type="text" class="card_input" style="margin:0px; width: 262px !important;"
                                                v-model="lcnclcont" ref="lcnclcont" v-bind:class="{add_opacity: !cancelView}">
                                            <label v-if="partial" style="margin-right: 5px;"><input type="radio" v-model="rg6" value="A"> 전체</label>
                                          
                                            <input v-if="!partial" type="text" class="card_input" style="margin:0px; width: 262px !important; "
                                                v-model="lcnclcont" ref="lcnclcont" v-bind:class="{add_opacity: !cancelView}">
                                        </div>
                                            
                                    </div>
                                    <!-- 카드결제 -->
                                    <div class="order_content_left"
                                        style="margin-right: 0px; float:left;" v-if="strcfrmtype == '0103'">
                                        <div style="padding: 3px;">
                                            <span class="card_label gray_bold">카드번호</span>
                                            <input type="text" class="card_input" v-model="cardno1" ref="cardno1" @keypress="onlyNumber">
                                            <input type="text" class="card_input" v-model="cardno2" ref="cardno2" @keypress="onlyNumber">
                                            <input type="text" class="card_input" v-model="cardno3" ref="cardno3" @keypress="onlyNumber">
                                            <input type="text" class="card_input" v-model="cardno4" ref="cardno4" @keypress="onlyNumber">
                                            <span v-if="CARD_COM!=''" type="text" style="width:100px; font-size:12px; margin-left:2px;"> 
                                                {{CARD_COM}}</span>
                                            <!--<select class="card_select" v-model="CARD_COM">
                                                <option v-for="(item, idx) in cardsa" v-bind:key="idx"
                                                    :value="item.data">{{ item.label }}</option>
                                            </select>-->
                                            <button class="new_btn"
                                                style="" @click="cardNumCheckBtn">카드번호체크</button>
                                        </div>
                                        <div style="padding: 3px; ">
                                            <span class="card_label gray_bold">유효기간</span>
                                            <input type="text" class="card_input" v-model="expdate1" ref="expdate1" @keypress="onlyNumber">월 &nbsp;
                                            <input type="text" class="card_input" v-model="expdate2" ref="expdate2" @keypress="onlyNumber">년
                                        <!-- </div>
                                        <div style="padding: 3px; "> -->
                                            <span class="card_label gray_bold" style="margin-left:15px;">할부선택</span>
                                            <select class="card_select" style="width: 60px !important;" v-model="HALBU">
                                                <option v-for="(item, idx) in halbus" v-bind:key="idx"
                                                    :value="item.data">{{ item.label }}</option>
                                            </select>
                                        </div>
                                        <div style="padding: 3px; ">
                                        <button class="order_Quantity"
                                            style="font-size: 12px; height: 25px; width:100%; margin-left:5px; ">단말기</button>
                                        </div>
                                            <div style="padding:3px;">
                                            <span class="card_label gray_bold">결제상태</span>
                                            <span>{{ ordermsg0 }}<br v-if="ordermsg != ''">{{ ordermsg }}</span>
                                        </div>
                                        <div style="padding: 3px;">
                                            <span class="card_label gray_bold">취소사유</span>
                                            <input v-if="partial" type="text" class="card_input" style="width: 266px !important; margin:0px;"
                                                v-model="lcnclcont" ref="lcnclcont" v-bind:class="{add_opacity: !cancelView}">
                                            <label v-if="partial" style="margin-right: 5px;"><input type="radio" v-model="rg6" value="A"> 전체</label>
                                            <!-- <label v-if="partial"><input type="radio" v-model="rg6" value="B"> 부분</label>-->

                                            <input v-if="!partial" type="text" class="card_input" style="width: 266px !important; margin:0px;"
                                                v-model="lcnclcont" ref="lcnclcont" v-bind:class="{add_opacity: !cancelView}">
                                        </div>
                                    </div>
                                    <!-- 현금결제 -->
                                    <div class="order_content_left" style="float:left; margin-right: 0px;" v-if="strcfrmtype == '0104'">
                                        <div  style="display:flex; margin:3px 0;">
                                            <span style="color:rgb(58, 62, 65);font-weight:bold; padding-top:5px; margin-right: 15px;  margin-bottom:5px;">
                                                현금영수증</span>
                                            <div class="input_label" style="margin-top:2px; margin-bottom:2px;">
                                                <label  ><input type="radio" style=""
                                                        value="9399" v-model="receiptTypeRadio"> <span style="">발행</span></label>
                                                <label style=""  ><input type="radio"style=""
                                                        value="9400" v-model="receiptTypeRadio" ><span style="">미발행</span></label>
                                            </div>
                                        </div>
                                        <div v-if="receiptTypeRadio == 9399" style="padding-left:2px; width:335px;">
                                            <div style="padding: 7px 0px 0px 3px; border-radius:0px; border-top: 1px solid rgb(58, 62, 65); display:flex;">
                                                <span class="card_label gray_bold" style="padding-top:5px; margin-right:10px;">발급용도</span>
                                                <div class="input_label" style="margin-top:1px; ">
                                                    <label style=""><input type="radio" style=""
                                                            value="9401" v-model="receiptTypeRadio2">
                                                        개인소득공제</label>
                                                    <label style=""><input type="radio" style=""
                                                            value="9402" v-model="receiptTypeRadio2">
                                                        사업자증빙</label>
                                                </div>
                                            </div>

                                            <div style="padding: 3px; padding-right:0px;">
                                                <span class="card_label gray_bold">발급번호</span>
                                                <select class="card_select" style="width: 90px !important; text-align:center; " v-model="ARR_PAYME" v-if="this.CTEMP != 'ORDER' || orderstate == '0201' || orderstate == '0203'">
                                                    <option>핸드폰번호</option>
                                                    <option>주민등록번호</option>
                                                    <option>사업자번호</option>
                                                </select>
                                                <input class="custom_input" type="text" style="font-size: 12px; height: 23px; width: 170px;" v-model="PAYMV1" @keypress="onlyNumber" v-if="this.CTEMP != 'ORDER' || orderstate == '0201' || orderstate == '0203'">
                                            </div>
                                            <div style="padding: 3px;">
                                                <span class="card_label gray_bold">결제상태</span>
                                                <span>{{ ordermsg0 }}<br v-if="ordermsg != ''">{{ ordermsg }}</span>
                                            </div>
                                            <div style="padding: 3px;">
                                                <span class="card_label gray_bold">취소사유</span>
                                                <input type="text" class="card_input" style="width: calc(100% - 75px) !important; margin:0px;"
                                                 v-model="lcnclcont" v-bind:class="{add_opacity: !cancelView}" ref="lcnclcont" >
                                            </div>
                                        </div>
                                    <!-- 현금결제 -->
                                    </div>
                                <!-- 결제부분 -->
                                </div>
                                <!-- 영수증전송 -->
                                <div class="" style=" float:right; padding:0px; width:290px; margin:0px;">
                                    <div style=" width:100%; margin-bottom: 10px; padding-bottom: 9px; font-size: 14px; border-bottom:1px solid var(--color-tp-text-black); font-weight: bolder;  color: var(--color-tp-text-black); ">
                                        영수증전송</div>
                                    <div style="margin:25px 71px 10px;"  class="input_label"
                                        v-bind:class="{add_opacity: receiptCardType}">
                                        <label><input type="radio" name="receiptSendRadio" v-model="U02"
                                                value="Email" :disabled="receiptCardType">Email</label>
                                        &nbsp;&nbsp;
                                        <label style=""><input style=""
                                                type="radio" name="receiptSendRadio" v-model="U02" value="SMS"
                                                :disabled="receiptCardType">SMS</label>
                                    </div>
                                  
                                        <p style="margin-bottom: 10px;" >
                                            <input type="text" class="custom_input" style="width:90%; margin-left:15px; margin-bottom:5px;" v-if="U02 == 'Email'"
                                                v-model="recEmail" ref="recEmail" :disabled="receiptCardType" placeholder="Email">
                                            <input type="text" class="custom_input" style=" width:90%; margin-left:15px;  margin-bottom:5px;" v-if="U02 == 'SMS'"
                                                v-model="recTel" ref="recTel" :disabled="receiptCardType" @keypress="onlyNumber" placeholder="전화번호">
                                        </p>
                                        <p style="display: flex; "  v-bind:class="{add_opacity: CTEMP != 'ORDER'}">
                                            <button style=""
                                                class="btn_see" :disabled="receiptCardType" @click="receiptpop">보기</button>
                                            <button style="" class="btn_send"
                                                :disabled="receiptCardType" @click="Certify('REC_SMS')">전송</button>
                                        </p>
                                  
                                    <!-- 영수증전송 -->
                                </div>
                                    <!-- <p style="height:30px; border-bottom: 1px solid #f1f1f1; color: #acacac; width: 40px;"></p> -->
                            <!-- 결제+영수증 -->
                            </div>
                           
              </div>
                    <!-- 고객상세내역 팝업창 -->
                    <div class="order_content_left" v-if="viewType=='customer'">
                        <div style="border-radius: 10px; background: #006bc9; width:635px; height: 573px; padding: 5px;">
                            <p style="color: white; font-size: 17px; padding-top: 10px; padding-left: 10px; font-weight: bold; margin-bottom: 0px;">
                                고객정보
                                <a style='float: right; font-size: 17px; margin-right: 10px;'
                                    @click="viewType='grid'"><i class="fas fa-times" style="color: white;"></i></a>
                            </p>
                            <div style="background: white; border-bottom-left-radius: 8px; border-bottom-right-radius: 8px; padding: 20px 5px 10px 5px; margin-top:10px;"
                                class="flex_column">
                                <div class="flex_row" style="margin-top: 10px; margin-bottom: 5px;">
                                    <span class="cust_label">고객유형</span>
                                    <label  style="margin-right: 10px;"><input type="radio" v-model="custType"
                                            value="R001" :disabled='true'> 일반개인</label>
                                    <label><input type="radio" v-model="custType" value="R002" :disabled='true'>
                                        사업자(개인/법인)</label>
                                </div>
                                <div class="flex_row" style="margin-bottom: 10px;">
                                    <span class="cust_label">고객번호</span>
                                    <input type="text" class="cust_input" v-model="tcustno">
                                    <span class="cust_label">고객명</span>
                                    <input type="text" class="cust_input" v-model="tcustnm">
                                </div>
                                <div class="flex_row" style="margin-bottom: 10px;">
                                    <span class="cust_label">전화번호</span>
                                    <input type="text" class="cust_input" v-model="tcusttel">
                                    <span class="cust_label">이메일</span>
                                    <input type="text" class="cust_input" v-model="tcustemail">
                                </div>
                                <div class="flex_row" style="margin-bottom: 10px;">
                                    <span class="cust_label">주소</span>
                                    <input type="text" class="cust_input" style="width: 450px;" v-model="addr">
                                </div>
                                <div class="flex_row" style="margin-bottom: 10px;">
                                    <span class="cust_label">메모</span>
                                    <input type="text" class="cust_input" style="width: 450px;" v-model="memo">
                                </div>
                                <div class="flex_row" style="margin-bottom: 10px; margin-top:5px;">
                                    <span class="cust_label">특이사항</span>
                                    <input type="text" class="cust_input" style="width: 450px;" v-model="tcont">
                                </div>
                                <!-- <div style="text-align: center; margin-bottom: 10px">
                                    <img src="../../../assets/btn_modify.png" alt="btn_modify">
                                </div> -->
                                <span class="gray_bold" style="margin-top:10px;">최근 3개월간 거래내역</span>
                                <ag-grid-vue class="ag-theme-alpine"
                                    style="width: 100%; height: 110px; margin: 8px 0px;" :columnDefs="gridHeader2"
                                    :rowData="camove" :gridOptions="gridOptions2" :headerHeight="25" :rowHeight="25"
                                    :suppressRowClickSelection="true" :rowSelection="rowSelection"
                                    :localeText="localeText">
                                </ag-grid-vue>
                                <span class="gray_bold" style="margin-top:10px;">주문 변동사항</span>
                                <ag-grid-vue class="ag-theme-alpine"
                                    style="width: 100%; height: 110px; margin: 8px 0px;" :columnDefs="gridHeader3"
                                    :rowData="caLog" :gridOptions="gridOptions3" :headerHeight="25" :rowHeight="25"
                                    :suppressRowClickSelection="true" :rowSelection="rowSelection"
                                    :localeText="localeText">
                                </ag-grid-vue>
                            </div>
                        </div>
                    </div>
                <!-- 왼쪽 -->
                </div>
                    <!-- 오른쪽 -->
                <div class="order_content_right" style="width: 370px; float: left;">

                        
                            <div class="flex_row" style="justify-content:space-between;border-bottom:1px solid  rgb(58, 62, 65);padding:0px 10px;"
                             v-bind:class="{add_opacity: !buttonView}">
                                <span style="font-weight: bold; font-size: 20px; padding-top:5px;">주문내역</span>
                                <div style="display:flex; justify-content:space-between; margin-bottom:10px;">
                                    <button style="margin-right:15px; " class="order_Quantity"  @click="updateCount('-')">-</button>
                                    <button style=" margin-right:23px; background:#ffffff; color: rgb(0, 107, 201)" class="order_Quantity" @click="updateCount('+')">+</button>
                                    <button  style="background:#ffffff; border-radius: 20px; width: 70px; height: 30px; color: #393C3F !important; 
                                    font-size: 13px; line-height: 19px; letter-spacing: 0.01em; text-align: center; border: 1px solid rgba(57, 60, 63, 0.15) !important;" @click="delList()">삭제</button></div>
                            </div>  
                            <!-- 선택 상품 목록 -->
                            <ag-grid-vue class="ag-theme-alpine" style="width: 370px; height: 170px;"
                                :columnDefs="gridHeader" :rowData="acDG" :gridOptions="gridOptions" :headerHeight="35"
                                :rowHeight="25" :suppressRowClickSelection="true" :rowSelection="rowSelection"
                                :localeText="localeText">
                            </ag-grid-vue>
                            <p class="order_payment" style="">
                                <span class="order_payment_text" style="">주문금액</span>
                                <span class="order_payment_price" style="">
                                    <span class="price" style="margin-right: 10px; font-size: 20px; font-weight: bold; ">{{ makeComma(span_totmoney) }}</span> 
                                    <span style="line-height: 21px; font-weight:bold">원</span>
                                </span>
                            </p>

                            <!-- 선택 상품 금액 상세_after -->
                            <div class="flex_column" style="border: 1px solid rgb(238, 238, 238); border-radius:5px; padding: 10px 0px 0px 0px;">
                                <div class="flex_row" style="margin-bottom: 2px;">
                                    <span class="price_label" style="padding-top: 8px;">과세방식</span>
                                    <div style="margin-top: 2px;">
                                        <select v-model="taxTypeSelect" class="custom_select" style="width:187px; height:28px; text-align:center;" v-bind:class="{add_opacity: !listView}">
                                            <option v-for="(item, index) in catax" v-bind:key="index" :value="item.data">
                                                {{ item.label }}
                                            </option>
                                        </select>
                                    </div>                                        
                                </div>
                                <div class="flex_row" style="margin-bottom: 2px;">
                                    <span class="price_label" style="padding-top: 8px;">
                                        비과세
                                        <div class="btn_select" @click="taxToTaxfree" v-bind:class="{add_opacity: !listView}" >*선택입력</div>
                                    </span>
                                    <div style="margin-top: 2px;">
                                        <input  type="text" style="text-align: right;" class="info_input" v-model="taxfree_span" ref="taxfree" v-bind:class="{add_taxfree: taxfreeInput}" :disabled="!taxfreeInput || !listView" @keypress="onlyNumber">
                                        &nbsp;<span>원</span>
                                    </div>                                        
                                </div>
                                <div class="flex_row" style="margin-bottom: 2px;">
                                    <span class="price_label" style="padding-top: 8px;">
                                        과세
                                    </span>
                                    <div style="margin-top: 2px;">
                                        <input type="text" style="text-align: right;" class="info_input"v-model="tax_span" ref="tax" readonly>
                                        &nbsp;<span>원</span>
                                    </div>                                        
                                </div>
                                <div class="flex_row" style="margin-bottom: 2px;">
                                    <span class="price_label" style="padding-top: 8px;">
                                        부가세
                                    </span>
                                    <div style="margin-top: 2px;">
                                        <input type="text" style="text-align: right;" class="info_input" v-model="surtax_span" ref="surtax" readonly>
                                        &nbsp;<span>원</span>
                                    </div>                                        
                                </div>
                                <div class="flex_row" style="margin-bottom: 10px;">
                                    <span class="price_label" style="padding-top: 8px;">
                                        할인금액
                                    </span>
                                    <div style="margin-top: 2px;">
                                        <input type="text" style="text-align: right;" class="info_input add_taxfree" v-model="span_sale" @keypress="onlyNumber" :disabled="!listView">
                                        &nbsp;<span>원</span>
                                    </div>                                        
                                </div>

                                <p class="order_payment" style="border:none; margin-bottom:0px;">
                                    <span class="order_payment_text" style="">총 결제금액</span>
                                    <span class="order_payment_price" style="">
                                        <span class="price" style="color: red;">{{ runmoney }}</span>
                                        <span style="line-height:21px; font-weight:bold">원</span>
                                    </span>
                                </p>
                            </div>

                            <div class="flex_row" style="font-size: 14px; margin-top:10px;">
                                <span class="info_label gray_bold" style="padding-top: 8px; margin-right:5px;">특이사항</span>
                                <input type="text" class="info_input" style="width:300px; " v-model="ocont" :disabled="!listView">
                            </div>

                            <!-- 선택 상품 금액 상세_before -->
                            <!-- <div class="flex_row" style="border: 1px solid #84ADE9; font-size: 14px;">
                                <span class="info_label gray_bold">구매금액</span>
                                <span
                                    style="display: inline-block; flex: 1; padding: 5px; text-align: right; font-weight: bold">{{
                                    makeComma(span_totmoney) }} 원</span>
                            </div> -->

                            <!-- <div class="flex_row" style="border: 1px solid #84ADE9; font-size: 14px;">
                                <span class="info_label gray_bold" style="padding-top: 8px;">할인금액</span>
                                <input type="text"
                                    style="flex: 1; padding: 5px; text-align: right; font-weight: bold; background: #DEDEDE; border: none;"
                                    v-model="span_sale">
                                <span
                                    style="display: inline-block; padding: 8px 5px 8px 0px; text-align: right; font-weight: bold; background: #DEDEDE;">
                                    원</span>
                            </div> -->

                            <!-- <div class="flex_row" style="border: 1px solid #979797; border-top: none; font-size: 14px;">
                                <span class="info_label gray_bold" style="padding-top: 22px;">과세내역</span>
                                <div style="flex: 1; font-size: 13px;">
                                    <div style="background: #CCCCCC; padding: 3px;">
                                        과세방식
                                        <select v-model="taxTypeSelect">
                                            <option value="9103">부가세포함</option>
                                            <option value="9102">부가세별도</option>
                                            <option value="9105">비부세포함</option>
                                            <option value="9101">비부세별도</option>
                                            <option value="9106">면세</option>
                                        </select>
                                    </div>
                                    <div style="width: 100%; background: #A9A9A9" class="flex_row">
                                        <div style="flex: 1; margin-right: 1px;">
                                            <span
                                                style="display: inline-block; width: 100%; background: #EEEEEE; text-align: center;">비과세</span>
                                            <input type="text" style="width:100%; text-align: right; border: none;"
                                                v-model="taxfree" ref="taxfree">
                                        </div>
                                        <div style="flex: 1; margin-right: 1px;">
                                            <span
                                                style="display: inline-block; width: 100%; background: #EEEEEE; text-align: center;">과세</span>
                                            <input type="text" style="width:100%; text-align: right; border: none;"
                                                v-model="tax" ref="tax">
                                        </div>
                                        <div style="flex: 1;">
                                            <span
                                                style="display: inline-block; width: 100%; background: #EEEEEE; text-align: center;">부가세</span>
                                            <input type="text" style="width:100%; text-align: right; border: none;"
                                                v-model="surtax" ref="surtax">
                                        </div>

                                    </div>
                                </div>
                            </div> -->


                            <!-- <div class="flex_row" style="border: 1px solid #84ADE9; font-size: 14px;">
                                <span class="info_label gray_bold" style="padding-top: 8px;">결제금액</span>
                                <input type="text"
                                    style="flex: 1; padding: 3px 0px 5px 5px; text-align: right; font-weight: bold; color: red; border: none; font-size: 17px;"
                                    v-model="runmoney">
                                <span
                                    style="display: inline-block; padding: 8px 5px 0px 8px; text-align: right; font-weight: bold; color: red; font-size: 17px;">원</span>
                            </div> -->

                            <!-- <div class="flex_row" style="border: 1px solid #84ADE9; font-size: 14px;">
                                <span class="info_label gray_bold" style="padding-top: 8px;">특이사항</span>
                                <input type="text"
                                    style="flex: 1; padding: 5px; text-align: left; font-weight: bold; border: none;"
                                    v-model="ocont">
                            </div> -->

                      
                        <!-- 금액옵션버튼 -->
                    
                        <!-- 결제옵션 버튼 -->
                        <div class="flex_row" style="margin-top:10px;">
                            <button class="bot_btn_on" v-bind:class="{bot_btn_off: botbtn1, add_opacity: payingState}" :disabled="botbtn1"
                                style="flex: 1; margin-right: 5px;" @click="sessionChk('ftranSend0')">결제대기</button>
                            <button class="bot_btn_on" v-bind:class="{bot_btn_off: botbtn5 || strcfrmtype=='0104', add_opacity: payingState}" :disabled="botbtn5"
                                style="flex: 1; margin-right: 5px;" @click="sessionChk('ftranSend6')">결제창전송</button>
                            <button class="bot_btn_on" v-bind:class="{bot_btn_off: botbtn3, add_opacity: payingState}" :disabled="botbtn3"
                                style="flex: 1; margin-right: 5px;" @click="sessionChk('ftranSend3')">결제수정</button>
                            <button class="bot_btn_on" v-bind:class="{bot_btn_off: botbtn4 || !cancelView, add_opacity: payingState}" :disabled="botbtn4 || !cancelView"
                                style="flex: 1;" @click="sessionChk('checkCancel')">결제취소</button>
                        </div>
                </div>
                <!-- 내용부분 -->
                
            </div>
            <span
                    style="font-size: 12px; margin-left: 10px; color: #006BC9;">
                    ※ 카드결제시 카드사에서 발송되는 문자에는 가맹점상호명 대신 (주)온카드 또는 PG사명으로 표시되오니 고객에게 공지 바랍니다.
                </span>
        </div>
        <OrderCustSelect v-on:close="closeCustSelectPop"></OrderCustSelect>
        <PartialCancel v-on:close="closePartialCancelPop"></PartialCancel>
    </modal>

</template>
<script>
    /*eslint-disable*/

    import Vue from 'vue'
    import { AgGridVue } from 'ag-grid-vue';
    import OrderCustSelect from './OrderCustSelect.vue'
    import PartialCancel from './PartialCancel.vue';

    export default {
        components: {
            AgGridVue, OrderCustSelect, PartialCancel
        },
        data() {
            return {
                gridOptions: null,
                gridOptions2: null,
                gridOptions3: null,
                gridHeader2: [
                    { headerName: '구매일자', field: 'orderdate', width: 100 },
                    { headerName: '구분', field: 'orderstatecodnm', width: 120 },
                    { headerName: '상품명', field: 'spointnm', width: 150 },
                    { headerName: '금액', field: 'totmoney', width: 100 },
                    { headerName: '특이사항', field: 'cont', width: 180 },
                ],
                gridHeader3: [
                    { headerName: '구매일자', field: 'wdate', width: 100 },
                    { headerName: '구분', field: 'gubun', width: 120 },
                    { headerName: '내역 및 취소사유', field: 'contents', width: 180 },
                    { headerName: '상품금액', field: 'totmoney', width: 100 },
                    { headerName: '승인총액', field: 'runmoney', width: 100 },
                    { headerName: '과세', field: 'tax', width: 100 },
                    { headerName: '부가세', field: 'surtax', width: 100 },
                    { headerName: '비과세', field: 'taxfree', width: 100 },
                    { headerName: '변경자', field: 'rcsid', width: 100 },
                ],
                localeText: { noRowsToShow: "상품을 선택해주세요." },
                gridHeader: [
                    {
                        headerName: '',
                        field: 'clicked',
                        editable: true,
                        width: 40,
                        headerCheckboxSelection: true,
                        checkboxSelection: true,
                    },
                    { headerName: '상품명', field: 'v_nam', width: 150 },
                    { headerName: '', field: 'count', hide: true },
                    { headerName: '', field: 'tot_price', hide: true},
                    { headerName: '', field: 'rt_pay', hide: true },
                    { headerName: '수량', field: 'count_span', width: 80 },
                    { headerName: '금액', field: 'tot_price_span', width: 100 },
                    { headerName: '단가', field: 'rt_pay_span', width: 100 }
                ],
                rowSelection: null,
                groupStartPos: 0, // 대분류 리스트에서 어느 idx부터 조회하는지 저장하는 변수
                groupBtnLimit: 5, // 대분류 리스트에 보여줄 개수
                srchOps: [
                { label: "전화번호", data: "070101" },
                { label: "고객번호", data: "070102" },
                { label: "이름", data: "070103" },
                { label: "이메일", data: "070106" }
            ],
                cardsa: [
                    { label: "카드사선택", data: "" },
                    { label: "하나(외환)", data: "하나(외환)" },
                    { label: "롯데", data: "롯데" },
                    { label: "현대", data: "현대" },
                    { label: "국민", data: "국민" },
                    { label: "BC", data: "BC" },
                    { label: "우리", data: "우리" },
                    { label: "삼성", data: "삼성" },
                    { label: "신한", data: "신한" },
                    { label: "한미", data: "한미" },
                    { label: "NH", data: "NH" },
                    { label: "하나카드", data: "하나카드" }
                ],

                ///// 여기부터 팝업 닫을 때 초기화 작업 완료 /////
                itemList: [], // 전체 상품 목록
                filteredList: [], // 좌측에 필터링된 리스트
                acDG: [], // 선택된 상품 목록(우측에 표기)
                groupList: [], // 대분류 리스트 
                groupBtn: [], // 대분류 리스트 중 화면에 표기할 5개 항목 리스트                
                groupClicked: "전체메뉴",
                srchField0: "0401",
                srchField: "070101",
                srchWord: "",
                camove: [],
                caLog: [],
                orderno: "",
                searchStr: "", // 상품검색키워드
                ARR_PAYME: "핸드폰번호",
                PAYMV1: "", // 발급번호
                halbus: [],
                HALBU: "00",
                CTEMP: "NOCUST",   //오더구분  :  신규팝업오더 : NOCUST  신규팝업+고객검색 :CUST  오더클릭 :ORDER
                CARD_COM: "",
                BCUM01: false,
                viewType: "grid",
                custname: "",
                inputTypeRadio: "메뉴그룹사용",
                inputTypeRadio2: "카드결제",
                strcfrmtype: "0103",
                receiptTypeRadio: "9400",
                receiptTypeRadio2: "",
                U02: "SMS",
                urlTel: "",
                RSELECT: "N",
                RSELECTID: "0",
                receiptCardType: false,
                botbtn1: false, // B01
                botbtn2: false, // B02
                botbtn3: false, // B03
                botbtn4: false, // B04
                botbtn5: false,
                span_totmoney: 0,
                discountPrice: 0,
                span_sale: "0",
                payPrice: 0,
                runmoney: "0",
                taxTypeSelect: "9103",
                catax: [],
                selfView: false,
                ocont: "",
                tax: 0, // 과세
                surtax: 0, // 부가세
                taxfree: 0, // 면세
                tax_span: '0',
                surtax_span: '0',
                taxfree_span: '0',
                selfItemName: "",
                selfCount: 1,
                selfCount_span: "1",
                selfUnitPrice: 0,
                selfUnitPrice_span: "0",
                selfPrice: 0,
                selfPrice_span: "0",
                tcustno: "",
                tcustnm: "",
                tcusttel: "",
                strtmp: "",
                tcont: "",
                custnm: "",
                custtel: "",
                custType: "",
                cfcompno: "",
                custemail: "",
                tcustemail: "",
                custid: "",
                custno: "",
                CUSTCHECK: "N",
                COMCODE: "NNNNNNNNNN",
                COMCODE01: "N",
                GSMSYN: "Y",
                GSVAL: "0@0@0@0@0",
                orderstate: "",
                strVianm: "",
                urlTel: "",
                urlEmail: "",
                recEmail: "",
                recTel: "",
                cardno1: "",
                cardno2: "",
                cardno3: "",
                cardno4: "",
                expdate1: "",
                expdate2: "",
                tid: "",
                ordermsg0: "",
                ordermsg: "",
                lcnclcont: "",
                GURLSMSVAL: "N", // 결제창 SMS , 이메일 전송
                addr: "",
                memo: "",
                tfast_join: "",
                GSANGNAME: "",
                POPCARD: "N",
                custtelenabled: false,
                custView: true,
                itemView: true,
                listView: true,
                buttonView: true,
                payCheckView: true,
                payResendView: false,
                cancelView: true,
                srchOk: false,
                taxfreeInput: false,
                payingState: false, // 결제 관련 버튼 클릭시 버튼 막을때 사용하는 변수
                newOverlap: "",
                newOrder: "",
                companyName: "",
                payButtonCheck: "",
                rg6: "A",
                GPCSID: "",
                partial: false,
                GMNM: "",
                COMKEY: "",
                unpublish: true
                ///// 여기부터 팝업 닫을 때 초기화 작업 완료 /////

            }
        },
        watch: {
            taxTypeSelect: function(val, oldVal) { // 과세방식 변경 리스너
                //# 최초에 팝업 뜰때도 나와서 주석처리 추후 기능 수정할것
                // this.$swal({
                //     html: "<span class='alert_txt'>부가세 계산방식이 변경되었습니다.<br>확인 바랍니다.</span>",
                //     showCloseButton: false,
                //     showCancelButton: false,
                //     confirmButtonText: '확인',
                //     focusConfirm: false,
                //     showLoaderOnConfirm: true
                // });
                switch(this.taxTypeSelect) {
                    case "9103": // 부가세포함
                        this.taxfree = 0;
                        this.taxfreeInput = false;
                        break;
                    case "9102": // 부가세별도
                        this.taxfree = 0;
                        this.taxfreeInput = false;
                        break;
                    case "9105": // 비부세포함
                        this.taxfreeInput = true;
                        break;
                    case "9101": // 비부세별도
                        this.taxfreeInput = true;
                        break;
                    case "9106": // 면세
                        this.taxfreeInput = false;
                        break;
                }
                this.calcPrice();
            },
            taxfree_span: function(val, oldVal) { // 비과세 변경 리스너
                if (val == '') {
                    val = '0';
                }
                console.log(this.span_totmoney);
                if(Number(this.span_totmoney) < Number(this.removeComma(val))) {
                    val = '0';
                    this.showAlert("[비과세 입력 오류]<br>비과세 금액이 구매금액보다 커서는 안됩니다.<br>다시 입력해 주세요.");
                }
                this.taxfree = this.removeComma(val);
                this.calcPrice();
            },
            span_sale: function (val, oldVal) { // 할인금액 변경 리스너
                if (val == '') {
                    val = '0';
                }
                if(Number(this.span_totmoney) < Number(this.removeComma(val))) {
                    val = '0';
                    this.showAlert("[할인금액 입력 오류]<br>할인금액이 구매금액보다 커서는 안됩니다.<br>다시 입력해 주세요.");
                }
                this.discountPrice = this.removeComma(val);
                this.span_sale = this.removeComma(val);
                this.calcPrice();
            },
            inputTypeRadio: function (val, oldVal) { // 상품입력방식 변경 리스너
                if (val == "메뉴그룹사용") {
                    this.groupBtnLimit = 5;
                    this.selfView = false;
                }
                if (val == "상품직접입력") {
                    this.groupBtnLimit = 3;
                    this.selfView = true;
                }
                this.updateGroupBtnList();
            },
            cardno1: function(val, oldVal) {
                if(this.cardno1.length >= 4) {
                    this.cardno1 = this.cardno1.substr(0, 4);
                    this.$refs.cardno2.focus();
                }
            },
            cardno2: function(val, oldVal) {
                if(this.cardno2.length >= 4) {
                    this.cardno2 = this.cardno2.substr(0, 4);
                    this.$refs.cardno3.focus();
                }
            },
            cardno3: function(val, oldVal) {
                if(this.cardno3.length >= 4) {
                    this.cardno3 = this.cardno3.substr(0, 4);
                    this.$refs.cardno4.focus();
                }
            },
            cardno4: function(val, oldVal) {
                if(this.cardno4.length >= 4) {
                    this.cardno4 = this.cardno4.substr(0, 4);
                    this.$refs.expdate1.focus();
                }
            },
            expdate1: function(val, oldVal) {
                if(this.expdate1.length >= 2) {
                    this.expdate1 = this.expdate1.substr(0, 2);
                    this.$refs.expdate2.focus();
                }
            },
            expdate2: function(val, oldVal) {
                if(this.expdate2.length >= 2) {
                    this.expdate2 = this.expdate2.substr(0, 2);
                }
            },
            srchField0: function (val, oldVal) { // ChangeCombo()
                if (this.srchField0 == "0401") {
                    this.srchOps = [
                        { label: "전화번호", data: "070101" },
                        { label: "고객번호", data: "070102" },
                        { label: "이름", data: "070103" },
                        { label: "이메일", data: "070106" }
                    ]
                    this.srchField = "070101";
                } else if (this.srchField0 == "0402") {
                    this.srchOps = [
                        { label: "대표전화번호", data: "070101" },
                        { label: "사업자번호", data: "070104" },
                        { label: "고객번호", data: "070102" },
                        { label: "회사명", data: "070103" },
                        { label: "대표자명", data: "070105" },
                        { label: "회사이메일", data: "070106" },
                        { label: "법인등록번호", data: "070107" },
                        { label: "담당자명", data: "070108" },
                        { label: "담당자연락처", data: "070109" },
                        { label: "담당자이메일", data: "070110" }
                    ]
                    this.srchField = "070101";
                } else if (this.srchField0 == "ALL") {
                    this.srchOps = [
                        { label: "전화번호", data: "ALL" }
                    ]
                    this.srchField = "ALL";
                }
            },
            selfCount_span(val, oldVal) {
                this.selfCount = val.replace(/,/g, '');
                this.selfPrice = Number(this.selfCount) * Number(this.selfUnitPrice);
                this.selfPrice_span = this.makeComma(this.selfPrice);
                this.selfCount_span = this.makeComma(this.selfCount);
            },
            selfUnitPrice_span(val, oldVal) {
                this.selfUnitPrice = val.replace(/,/g, '');
                this.selfPrice = Number(this.selfCount) * Number(this.selfUnitPrice);
                this.selfPrice_span = this.makeComma(this.selfPrice);
                this.selfUnitPrice_span = this.makeComma(this.selfUnitPrice);
                console.log(this.selfUnitPrice_span);
            },
            custnm(val) {
                this.custnm = this.custnm.replace(/[^0-9A-Za-zㄱ-ㅎ|ㅏ-ㅣ|가-힣()]/g, '');
            },
            custemail(val) {
                this.custemail = this.custemail.replace(/[^0-9A-Za-z.@\-_]/g, '');   
            },
            selfItemName(val) {
                this.selfItemName = this.selfItemName.replace(/[^0-9A-Za-zㄱ-ㅎ|ㅏ-ㅣ|가-힣()/\-_]/g, '');
            },
            srchWord(val) {
                this.srchWord = this.srchWord.replace(/[^0-9A-Za-zㄱ-ㅎ|ㅏ-ㅣ|가-힣()]/g, '');
            },
            lcnclcont(val) {
                this.lcnclcont = this.lcnclcont.replace(/[^0-9A-Za-zㄱ-ㅎ|ㅏ-ㅣ|가-힣()]/g, '');
            },
            addr: function (val) { //주소
                this.addr = this.addr.replace(/[^0-9A-Za-zㄱ-ㅎ|ㅏ-ㅣ|가-힣\.\,\=\~\{\}\[\]\:\@\$\#\!\^\<\>\*\(\)\-\ \_\+\|\?\/\ ]/g, '');
            },
             ocont: function (val) { //주소
                this.ocont = this.ocont.replace(/[^0-9A-Za-zㄱ-ㅎ|ㅏ-ㅣ|가-힣\.\,\=\~\{\}\[\]\:\@\$\#\!\^\<\>\*\(\)\-\ \_\+\|\?\/\ ]/g, '');
            },
        },
        methods: {
            closePartialCancelPop(value) { // closed_cancle
                if(value.doAction) {
                    this.sessionChk('sendData');
                }
                this.payingState = false;
            },
            closeCustSelectPop(value) {
                this.RSELECT = value.RSELECT;
                if(value.RSELECTID != "") {
                    this.RSELECTID = value.RSELECTID;
                }
                this.sessionChk('sendData1');
            },
            orderDelAll_Before() {
                var self = this;
                this.$swal({
                    html: "<span class='alert_txt'>[전체 상품 삭제]<br>전체 상품을 삭제 하시겠습니까?</span>",
                    showCloseButton: false,
                    showCancelButton: true,
                    cancelButtonText: '닫기',
                    confirmButtonText: '확인',
                    focusConfirm: false,
                    showLoaderOnConfirm: true
                }).then((result) => {
                    if (result.value) {
                        self.orderDelAll();
                    }
                })
            },
            orderDelAll() {
                this.acDG = [];
                for (var i = 0; i < this.filteredList.length; i++) {
                    this.filteredList[i].clicked = false;
                }
                this.gridOptions.api.redrawRows(); // ag-grid 데이터 업데이트해주기
                this.calcPrice();
            },
            beforeOpen(event) { // init1() 
                this.catax = [];
                for(var i=0; i<Vue.prototype.$default["catax"].length; i++) {
                    var item = (Vue.prototype.$default["catax"])[i];
                    this.catax.push({
                        label: item.label,
                        data: item.data
                    });
                }
                this.taxTypeSelect = this.catax[0].data;
                this.getData(); // 그리드 그리기

                //오더구분  :  신규팝업오더 : NOCUST  신규팝업+고객검색 :CUST  오더클릭 :ORDER
                this.CTEMP = event.params.CTEMP;
                this.orderno = event.params.orderno;

                // if ( ExternalInterface.available) {
                //     ExternalInterface.addCallback("FlexBCardCall",javaToflex);						
                // }


                this.HALBU_ADD();
                this.changMenuBtn("NEW", "btn02", "", 0, 0, 0, 0, 0);
                /*
                dataGroup1.dataProvider = FlexGlobals.topLevelApplication.GDATEMENU;  //메뉴				
                horizontalList.dataProvider = FlexGlobals.topLevelApplication.GDATEMENUGROUP; //메뉴그룹		
                Taxselect.dataProvider = FlexGlobals.topLevelApplication.GDATETAX; //관세방식	
                XMLTree1.dataProvider = FlexGlobals.topLevelApplication.GDATATREE; // dataset[0].LISTOK;
                COMCODE = FlexGlobals.topLevelApplication.GUFLD;   //카드사구분				
                listGrid.dataProvider = acDG;  //선택상품연결
                */

                if (this.CTEMP == "ORDER") {   //접수된 오더를 클릭했을 때
                    this.sessionChk('sendData');
                } else {     // 신규 접수 버튼클릭하고  고객검색 안했을 경우
                    this.CTEMP = "NOCUST";  //오더구분  접수창만 뛰우고 결제
                    // /*******카드창 영수증전송 화면******************/
                    this.enabledReceiptCard(false);  //카드영수증창 선택안되게
                    // /*******현금 영수증전송 화면******************/
                    this.enabledReceiptPay(false);  //현금영수증창 선택안되게
                    // /*******결제 버튼 처리******************/
                    this.enabledButton(true, true, false, false, true);  //우측버튼  표기  대기,결제,수정,취소,전송	
                }

                //대량업로드-전화번호중복-특허버젼-무조건신규-업체명-관리자만 현금영수증결제
                this.newOverlap = Vue.prototype.$ConfigOption.split('')[1];  // 중복신규버튼
                this.newOrder = Vue.prototype.$ConfigOption.split('')[3];   // 무조건신규버튼
                this.companyName = Vue.prototype.$ConfigOption.split('')[4];   // 업체명 표기
                this.payButtonCheck = Vue.prototype.$ConfigOption.split('')[5];   // 관리자만 현금영수증결제

                this.COMCODE = Vue.prototype.$default["shopinfo"].uselistfld;
                this.GMNM = Vue.prototype.$default["shopinfo"].mnm;
                this.COMKEY = Vue.prototype.$default["shopinfo"].comkey;
            },
            /*************************카드 영수증 전송창  화면처리 시작 ***********************/
            enabledReceiptCard(t1) {
                this.receiptCardType = !t1;
            },
            /*************************현금 영수증 전송창  화면처리 시작 ***********************/
            enabledReceiptPay(t1) {
                this.receiptCardType = !t1;
            },
            /*************************접수창 버튼 화면처리 시작 ***********************/
            enabledButton(t1, t2, t3, t4, t5) {
                this.botbtn1 = !t1; // 결제대기
                this.botbtn2 = !t2; // 즉시결제
                this.botbtn3 = !t3; // 수정
                this.botbtn4 = !t4; // 취소
                this.botbtn5 = !t5; // 결제창전송
            },
            /************************* 메뉴 버튼 처리  ************************************/
            changMenuBtn(msw, img, sVal, t1, t2, t3, t4, t5) {
                // var t;
                // if (msw == "NEW") {  //신규창 오픈시 메뉴	
                //     for(t = 0; t <  FlexGlobals.topLevelApplication.GDATEMENU.length; t++) {
                //         var tmpObject = new Object();
                //         tmpObject = FlexGlobals.topLevelApplication.GDATEMENU.getItemAt(t);					
                //         tmpObject.v_img = img;
                //         tmpObject.v_font1 = uint(0x033967);
                //         tmpObject.v_font2 = uint(0x555555);
                //         tmpObject.v_font3 = uint(0x4F4F4F);
                //         tmpObject.v_font4 = uint(0x555555);
                //         tmpObject.v_font5 = uint(0x555555);					
                //         FlexGlobals.topLevelApplication.GDATEMENU.setItemAt(tmpObject, t);						
                //     }
                // }else if (msw == "UNSELECT" ){  //선택된 메뉴  해제
                //     for( t = 0; t <  FlexGlobals.topLevelApplication.GDATEMENU.length; t++) {
                //         var tmpObject1 = new Object();
                //         tmpObject1 = FlexGlobals.topLevelApplication.GDATEMENU.getItemAt(t);					  
                //         if(String(tmpObject1["rt_scod"]) == sVal) {
                //             tmpObject1.v_img = img;
                //             tmpObject1.v_font1 = uint(0x033967);
                //             tmpObject1.v_font2 = uint(0x555555);
                //             tmpObject1.v_font3 = uint(0x4F4F4F);
                //             tmpObject1.v_font4 = uint(0x555555);
                //             tmpObject1.v_font5 = uint(0x555555);	
                //             FlexGlobals.topLevelApplication.GDATEMENU.setItemAt(tmpObject1, t);
                //         }	
                //     }
                // }else{  //선택된 메뉴 일때
                //     for( t = 0; t <  FlexGlobals.topLevelApplication.GDATEMENU.length; t++) {
                //         var tmpObject1 = new Object();
                //         tmpObject1 = FlexGlobals.topLevelApplication.GDATEMENU.getItemAt(t);					  
                //         if(String(tmpObject1["rt_scod"]) == sVal) {
                //             tmpObject1.v_img = img;
                //             tmpObject1.v_font1 = uint(0xffffff);
                //             tmpObject1.v_font2 = uint(0xffffff);
                //             tmpObject1.v_font3 = uint(0xffffff);
                //             tmpObject1.v_font4 = uint(0xffffff);
                //             tmpObject1.v_font5 = uint(0xffffff);	
                //             FlexGlobals.topLevelApplication.GDATEMENU.setItemAt(tmpObject1, t);
                //         }	
                //     }
                // }
            },
            HALBU_ADD() {
                this.halbus = [];
                this.halbus.push({ label: "일시불", data: "00" });
                var iTmp = Number(Vue.prototype.$default["shopinfo"].halbu)
                for (var i=2; i<iTmp + 1; i++) {
                    this.halbus.push({ label: i+"개월", data: this.make2Digit(i) });
                }
            },
            btVisible(m) {
                switch (m) {
                    case "TH":     // 카드승인  시작 		-- 버튼 감추기				
                        this.botbtn1 = false;
                        this.botbtn2 = false;
                        this.botbtn3 = false;
                        this.botbtn4 = false;
                        // LOAD1.text = "       결제 진행 중 입니다.               잠시만 기다려 주세요."
                        // LOAD2.visible = true;
                        break;
                    case "TV":     // 카드승인 끝/취소 			--  버튼보여주기			
                        this.botbtn2 = true;
                        this.botbtn3 = true;
                        this.botbtn4 = true;
                        // LOAD2.visible = false;
                        break;
                    case "FH":     // 취소  시작 		-- 버튼 감추기				
                        this.botbtn1 = false;
                        this.botbtn2 = false;
                        this.botbtn3 = false;
                        this.botbtn4 = false;
                        // LOAD1.text = "     결제취소 진행 중 입니다.        잠시만 기다려 주세요."
                        // LOAD2.visible = true;
                        break;
                    case "FV":     // 취소  끝/취소 	 			--  버튼보여주기			
                        this.botbtn1 = false;
                        this.botbtn2 = false;
                        this.botbtn3 = true;
                        this.botbtn4 = true;
                        // LOAD2.visible = false;
                        break;
                }
            },
            sessionChk: function (chkF) { // 서버 세션체크
                this.payingState = true;
                var params1 = new Object();
                params1.chkF = chkF;

                switch (chkF) {
                    case "sendData":
                        break;
                    case "sendData1":
                        break;
                    case "ftranSend0":	   //결제대기					
                        break;
                    case "ftranSend2":	  //즉시 결제
                        this.btVisible("TH");
                        break;
                    case "ftranSend3":
                        break;
                    case "ftranSend1":
                        break;
                    case "ftranSend6":	  //결제창전송
                        break;
                    case "checkCancel":
                        //this.btVisible("FH");
                        break;
                }

                // 세션체크 안함
                this.sessionChkOk(chkF);
                return;

                let self = this;
                let urls = Vue.prototype.$mainUrl + '/gate/__MDULES/FLEX21/f_xml_Session.asp';
                $.ajax({
                    url: urls,
                    dataType: 'text',
                    type: 'post',
                    data: {},
                    error: function (jqXHR, textStatus, errorThrown) { // sessionChk_faultHandler
                        self.showAlert("[fl1010][1251][세션체크]데이타전송중 또는 해당 웹페이지 오류. 다시 시도 바랍니다.");
                    },
                    success: function (res) {
                        let json = JSON.parse(res.replace(/\r/gi, '\\n').replace(/\n/gi, ' '));
                        if (event.result.result_set.record.session == "Y") {
                            self.sessionChkOk(chkF);
                        } else if (event.result.result_set.record.session == "N") {
                            // var urlop:String = "";
                            // var url:URLRequest;
                            // url = new URLRequest("/");	
                            // urlop = "_self";				
                            // navigateToURL(url, urlop);	
                            // urlop = null;
                            // url = null;
                        }
                    }
                });
            },
            sessionChkOk(chkF) { // sessionChk_resultHandler
                //console.log(chkF);
                switch (chkF) {
                    case "sendData":
                        this.sendData();
                        break;
                    case "sendData1":
                        this.sendData1();
                        break;
                    case "ftranSend0":      // 결제 대기
                        this.ftranSend('TR01', '10', '0');
                        break;
                    case "ftranSend2":
                        this.ftranSend('TR02', '20', '2');  //즉시 결제
                        break;
                    case "ftranSend3":
                        this.ftranSend('TR02', '30', '3'); // 수정
                        break;
                    case "ftranSend1":
                        this.ftranSend('TR02', '50', '1');
                        break;
                    case "ftranSend6":      // 결제창전송대기							
                        if (this.chkUrlSEND()) {
                            this.GURLSMSVAL = "Y"
                            this.ftranSend('TR01', '10', '0');
                        }
                        break;
                    case "checkCancel":
                        this.checkCancel();
                        break;
                    default:
                        this.payingState = false;
                        break;
                }
            },
            /*********************접수창 저장시 필드 체크 시작********************************************/
            sendCheck(checkBtn, checkOrd) {

                // 결제창전송 버튼 클릭시  체크
                if (this.GURLSMSVAL == "Y") {
                    if (this.U02 == "SMS") {   // sms
                        if (this.urlTel == "") {   // 전화번호를 
                            this.payingState = false;
                            this.$refs.urlTel.focus();
                            this.showAlert("SMS 발송용 전화번호를 입력해 주세요");
                            //urlTel.setFocus()
                            return false;
                        }
                    }

                    // if (U01.selected == true){   // 이메일
                    if (this.U02 == "Email") {   // 이메일
                        if (this.urlEmail == "") {   // 이메일을
                            this.payingState = false;
                            this.$refs.urlEmail.focus();
                            this.showAlert("발송용 이메일을 입력해 주세요");
                            return false;
                        }
                    }
                }

                // 고객정보 체크
                if (this.CTEMP != "NOCUST") {   // 고객 정보  검색  후  저장
                    if (this.custtel == "") {   // 고객 전화번호
                        this.payingState = false;
                        this.$refs.custtel.focus();
                        this.showAlert("전화번호를 입력해 주세요");
                        return false;
                    }
                    if (this.custnm == "") {   // 고객 명
                        this.payingState = false;
                        this.$refs.custnm.focus();
                        this.showAlert("고객명을 입력해 주세요");
                        return false;
                    }
                }

                // 주문 금액 관련 체크
                if (Number(this.runmoney.replace(/,/g, "")) < 999) {
                    this.payingState = false;
                    this.showAlert("상품총액은 1,000원 이상 되어야 합니다. 상품금액을 다시 입력해 주세요")
                    return false;
                    //# 잠시주석
                    // } else if (this.taxfree == "") {
                    //     this.$swal({
                    //         html:"<span class='alert_txt'>비과세를 입력해 주세요</span>",
                    //         showCloseButton: false,
                    //         showCancelButton: false,
                    //         confirmButtonText: '확인',
                    //         focusConfirm: false,
                    //         showLoaderOnConfirm: true
                    //     });
                    //     this.$refs.taxfree.focus();
                    //     return false;
                    // } else if (this.tax == "") {
                    //     this.$swal({
                    //         html:"<span class='alert_txt'>과세를 입력해 주세요</span>",
                    //         showCloseButton: false,
                    //         showCancelButton: false,
                    //         confirmButtonText: '확인',
                    //         focusConfirm: false,
                    //         showLoaderOnConfirm: true
                    //     });
                    //     this.$refs.tax.focus();
                    //     return false;
                    // } else if (this.surtax == "") {
                    //     this.$swal({
                    //         html:"<span class='alert_txt'>부가세를 입력해 주세요</span>",
                    //         showCloseButton: false,
                    //         showCancelButton: false,
                    //         confirmButtonText: '확인',
                    //         focusConfirm: false,
                    //         showLoaderOnConfirm: true
                    //     });
                    //     this.$refs.surtax.focus();
                    //     return false;
                } else if (this.span_totmoney == 0) {
                    this.payingState = false;
                    this.showAlert("총액을 입력해 주세요");
                    return false;
                }

                // 카드관련 체크
                if (this.strcfrmtype == "0103" && checkBtn == "TR02" && checkOrd == "20") {     //즉시결제 일때만 카드정보 체크
                    var strtmp = this.cardno1 + this.cardno2 + this.cardno3 + this.cardno4;
                    var strtmp2 = this.expdate1 + this.expdate2;
                    if (this.cardno1 == "") {
                        this.payingState = false;
                        this.$refs.cardno1.focus();
                        this.showAlert("카드번호를 입력해 주세요");
                        return false;
                    } else if (this.cardno2 == "") {
                        this.payingState = false;
                        this.$refs.cardno2.focus();
                        this.showAlert("카드번호를 입력해 주세요");
                        return false;
                    } else if (this.cardno3 == "") {
                        this.payingState = false;
                        this.$refs.cardno3.focus();
                        this.showAlert("카드번호를 입력해 주세요");
                        return false;
                    } else if (this.cardno4 == "") {
                        this.payingState = false;
                        this.$refs.cardno4.focus();
                        this.showAlert("카드번호를 입력해 주세요");
                        return false;
                    } else if (this.expdate1 == "") {
                        this.payingState = false;
                        this.$refs.expdate1.focus();
                        this.showAlert("유효기간을 입력해 주세요");
                        return false;
                    } else if (this.expdate2 == "") {
                        this.payingState = false;
                        this.$refs.expdate2.focus();
                        this.showAlert("유효기간을 입력해 주세요");
                        return false;
                    } else if (strtmp.length < 13) {
                        this.payingState = false;
                        this.$refs.cardno1.focus();
                        this.showAlert("카드번호는 14자 이상이어야 합니다.");
                        return false;
                    } else if (strtmp2.length != 4) {
                        this.payingState = false;
                        this.$refs.expdate1.focus();
                        this.showAlert("유효기간이 4자리가 아닙니다.");
                        return false;
                    }
                }
                return true;
            },
            checkCancel() {
                var params1 = new Object();
                if (this.lcnclcont == "") {
                    this.$refs.lcnclcont.focus();
                    //this.btVisible("FV");
                    this.payingState = false;
                    this.showAlert("취소사유를 넣어 주세요");
                } 
                else 
                {
                    this.$swal({
                    html: "<span class='alert_txt'>[결제 취소 확인]<br>결제를 취소 하시겠습니까?</span>",
                    showCloseButton: false,
                    showCancelButton: true,
                    cancelButtonText: '취소 안함',
                    confirmButtonText: '취소',
                    focusConfirm: false,
                    showLoaderOnConfirm: true
                    }).then((result) => { // alertCloseHandler
                        if (result.value) {
                                                    
                            let self = this;
                            let urls = Vue.prototype.$mainUrl;

                            if (this.orderstate == "0201" || this.orderstate == "0203") {   //승인대기, 승인요청중					
                                params1.mcact = "TR0202";
                                params1.ordernos = this.orderno;
                                params1.canmsg = this.lcnclcont;
                                urls = urls + "/gate/__MDULES/FLEX21/f_xml_re_batch.asp";

                            } else if (this.orderstate == "0206") {  //승인완료						
                                params1.cfrmtype = this.strcfrmtype;
                                params1.orderno = this.orderno;
                                params1.totpay = this.runmoney;
                                params1.span_tax = this.tax;
                                params1.span_surtax = this.surtax;
                                params1.span_taxfree = this.taxfree;

                                if (this.strcfrmtype == "0104") {
                                    params1.tid = this.tid.replace(/ /g, "");
                                    params1.cnclcont = this.lcnclcont;

                                    if (this.receiptTypeRadio == "9400") {
                                        urls = urls + "/gate/__MDULES/FLEX21/Pay_INIcancel_V10.asp";
                                    } else {							
                                            if (this.COMCODE.substr(0, 1) == "I") {
                                                urls = urls + "/gate/__MDULES/FLEX21/INIcancel_V10.asp";
                                            } else if (this.COMCODE.substr(0, 1) == "C") {
                                                urls = urls + "/gate/__MDULES/FLEX21/INIcancel_V10_give.asp";
                                            } else if (this.COMCODE.substr(0, 1) == "K") {
                                                urls = urls + "/gate/__MDULES/FLEX21/Kcp_Card_Cancel.asp";
                                            } else if (this.COMCODE.substr(0, 1) == "G") {
                                                urls = urls + "/gate/__MDULES/FLEX21/Kcp_Card_Cancel_give.asp";
                                            } else if (this.COMCODE.substr(0, 1) == "A") {
                                                urls = urls + "/gate/__MDULES/FLEX21/ALLAT_PAY_Cancel.asp";
                                            } else if (this.COMCODE.substr(0, 1) == "B" || this.COMCODE.substr(0, 1) == "F") {
                                                urls = urls + "/gate/__MDULES/FLEX21/ALLAT_PAY_Cancel_give.asp";
                                            } else if (this.COMCODE.substr(0, 1) == "N") {
                                                urls = urls + "/gate/__MDULES/FLEX21/Nice_Card_Cancel.asp";
                                            } else if (this.COMCODE.substr(0, 1) == "E") {
                                                urls = urls + "/gate/__MDULES/FLEX21/Nice_Card_Cancel_give.asp";
                                            } else if (this.COMCODE.substr(0, 1) == "D") {
                                                urls = urls + "/gate/__MDULES/FLEX21/DANAL_PAY_CANCEL.asp";
                                            } else if (this.COMCODE.substr(0, 1) == "H") {
                                                urls = urls + "/gate/__MDULES/FLEX21/DANAL_PAY_CANCEL_give.asp";
                                            }
                                    }

                                } else {
                                    params1.tid = this.tid.replace(/ /g, "");
                                    params1.cnclcont = this.lcnclcont;
                                    if(this.rg6 == "B") {
                                        params1.comcode = this.COMCODE;
                                        params1.Taxselect = this.taxTypeSelect
                                        params1.orgPayInfo = this.GPCSID;  //부분취소정보
                                        this.$modal.show('partial-cancel', {params: params1});
                                        return;
                                    } else {
                                        params1.cncPay = this.runmoney;      //총액
                                        params1.cncType = "A";     //전체취소
                                        if (this.COMCODE.substr(0, 1) == "I") {
                                            urls = urls + "/gate/__MDULES/FLEX21/INIcancel_V10.asp";
                                        } else if (this.COMCODE.substr(0, 1) == "C") {
                                            urls = urls + "/gate/__MDULES/FLEX21/INIcancel_V10_give.asp";
                                        } else if (this.COMCODE.substr(0, 1) == "K") {
                                            urls = urls + "/gate/__MDULES/FLEX21/Kcp_Card_Cancel.asp";
                                        } else if (this.COMCODE.substr(0, 1) == "G") {
                                            urls = urls + "/gate/__MDULES/FLEX21/Kcp_Card_Cancel_give.asp";
                                        } else if (this.COMCODE.substr(0, 1) == "A") {
                                            urls = urls + "/gate/__MDULES/FLEX21/ALLAT_Card_Cancel.asp";
                                        } else if (this.COMCODE.substr(0, 1) == "B" || this.COMCODE.substr(0, 1) == "F") {
                                            urls = urls + "/gate/__MDULES/FLEX21/ALLAT_Card_Cancel_give.asp";
                                        } else if (this.COMCODE.substr(0, 1) == "N") {
                                            urls = urls + "/gate/__MDULES/FLEX21/Nice_Card_Cancel.asp";
                                        } else if (this.COMCODE.substr(0, 1) == "E") {
                                            urls = urls + "/gate/__MDULES/FLEX21/Nice_Card_Cancel_give.asp";
                                        } else if (this.COMCODE.substr(0, 1) == "D") {
                                            urls = urls + "/gate/__MDULES/FLEX21/DANAL_CARD_CANCEL.asp";
                                        } else if (this.COMCODE.substr(0, 1) == "H") {
                                            urls = urls + "/gate/__MDULES/FLEX21/DANAL_CARD_CANCEL_give.asp";
                                        }
                                    }                               
                                }
                            }
                                //console.log(urls);
                                //console.log(params1);
                            $.ajax({
                                url: urls,
                                dataType: 'text',
                                type: 'post',
                                data: params1,
                                error: function (jqXHR, textStatus, errorThrown) { // faultHandler
                                    self.showAlert("[fl1010][1190][승인취소]데이타전송중 또는 해당 웹페이지 오류. 다시 시도 바랍니다.");
                                    self.payingState = false;
                                    self.btVisible("FV");
                                },
                                success: function (res) {
                           // console.log(res);
                                    try { // resultHandler
                                        let json = JSON.parse(res.replace(/\r/gi, '\\n').replace(/\n/gi, ' '));

                                        if (json["ResultOk"].ResultCode == "00") {
                                            self.showAlert("주문이 취소 되었습니다.");
                                        } else if (json["ResultOk"].ResultCode == "02") {
                                            self.showAlert("이니시스는 취소 되었으나 DB저장 중 에러 발생");
                                        } else if (json["ResultOk"].ResultCode == "01") {
                                            self.showAlert("카드 취소 실패 :" + json["ResultOk"].ResultMsg);
                                        } else if (json["ResultOk"].ResultCode == "03") {
                                            self.showAlert("주문이 취소되었습니다. 다시 시도해 주세요");
                                        }
                                        self.checkUser();
                                    } catch (error) {
                                        self.showAlert("JSON 파싱 에러");
                                        this.payingState = false;
                                    }
                                }
                            });
                        } else {  
                             this.payingState = false;                          
                            //Alert.show("이메일이 변경되지 않았습니다.");
                        }
                    })
                }
            },
            /***** 고객조회후 결과값 _결제대기, 결제하기, 결제창 전송 결제대기 *****/
            ftranSend(mcact, ordergbn, fast_join) {
                if (this.custemail != this.tcustemail) {		//이메일 변경 되었을 때			
                    this.$swal({
                        html: "<span class='alert_txt'>[이메일 변경 확인]<br>기존 저장된 이메일과 입력된 이메일이 일치하지 않습니다. 이메일을 변경하시겠습니까?</span>",
                        showCloseButton: false,
                        showCancelButton: true,
                        cancelButtonText: '닫기',
                        confirmButtonText: '확인',
                        focusConfirm: false,
                        showLoaderOnConfirm: true
                    }).then((result) => { // alertCloseHandler
                        if (result.value) {
                            this.tcustemail = this.custemail;
                            //Alert.show("이메일이 변경되었습니다.");
                        } else {
                            this.custemail = this.tcustemail;
                            //Alert.show("이메일이 변경되지 않았습니다.");
                        }
                    })
                    this.payingState = false;
                    this.btVisible("TV");
                } else { //이메일 변경안되었을 때					
                    var stemp = "";
                    var snametmp = "";
                    var isu = 0;
                    var ihap = 0;
                    if (this.sendCheck(mcact, ordergbn)) {   // 입력창  필드 체크
                        var params = new Object();

                        /************************ 고객 정보 *********************************/
                        if (this.CTEMP == "NOCUST") {  //고객 정보 없이 등록
                            params.f_cpo = "";       //고객구분  일반고객:0401  사업자고객:0402
                            params.f_ome = "0401";
                            params.custid = "1";       //고객ID
                            params.custno = "0000000001";       //고객번호									
                            params.custnm = "미등록고객";       //고객명										
                            params.custtel = "00000000000";    // 전화번호      					
                            params.custemail = "00@00.net";			//이메일	
                            params.addr = ""; //주소
                            if (this.strcfrmtype == "") {
                                // this.strcfrmtype = "0103";
                                this.strcfrmtype = "0102";
                                
                            }
                            // 주문번호 생성
                            var d = new Date();
                            var yyyy = d.getFullYear();
                            var MM = this.make2Digit(d.getMonth() + 1);
                            var dd = this.make2Digit(d.getDate());
                            var hh = this.make2Digit(d.getHours());
                            var mm = this.make2Digit(d.getMinutes());
                            var ss = this.make2Digit(d.getSeconds());
                            var tordernum = yyyy + MM + dd + hh + mm + ss;
                            var randNum = Math.floor(Math.random() * 10000000000) + 1;
                            tordernum = tordernum + randNum + "000";
                            this.orderno = tordernum.substr(0, 20);
                        } else {

                            if (this.srchField0 == "0402") {
                                params.f_cpo = this.cfcompno;       //사업자번호 고객구분  일반고객:0401  사업자고객:0402
                                params.f_ome = "0402";
                            } else {
                                params.f_cpo = "";
                                params.f_ome = "0401";
                            }
                            params.custid = this.custid;       //고객ID
                            params.custno = this.custno;       //고객번호									
                            params.custnm = this.custnm;       //고객명	
                            params.addr = this.addr;  //주소
                            params.custtel = this.custtel;         // 전화번호      
                            if (this.custemail == "") {    //이메일
                                params.custemail = "00@00.net";
                            } else {
                                params.custemail = this.custemail;
                            }
                        }
                        /*******************************************************************/

                        /*********************추가접수***********************************/
                        if (fast_join == "1") {                           //추가 접수시 1을 입력
                            params.orderno = "";
                            params.oldorderno = this.orderno;              // 예전 주문번호 
                        } else {
                            this.tfast_join = fast_join;
                            params.orderno = this.orderno;
                            params.oldorderno = "";
                        }
                        /****************************************************************************/


                        if (this.strcfrmtype == "0103" || this.strcfrmtype == "0102") {	//카드결제
                            /*********************카드결제정보***********************************/
                            params.cardno = this.cardno1 + this.cardno2 + this.cardno3 + this.cardno4;  //카드번호
                            params.cardtype = "";
                            params.expdate = this.expdate1 + "/" + this.expdate2;       //유효기간
                            params.driverno = this.HALBU;              // 할부기간	
                           // params.CardCompany = this.CARD_COM;     //카드사
                           params.CardCompany = "";
                            /********************************************************/
                            params.drivertel = "";
                            params.drivernm = "";
                            // if (cardAgree2.selected || cardAgree3.selected) {
                            //     params.cardAgree = "E";
                            // } else {
                            //     params.cardAgree = cardAgree.selectedValue;
                            // }
                        } else if (this.strcfrmtype == "0104") {   //핸드폰결제
                            /*********************현금결제정보***********************************/
                            params.drivernm = this.ARR_PAYME;
                            params.drivertel = this.receiptTypeRadio2;

                            /**************************************************************/
                            params.cardno = "";
                            params.cardtype = "";
                            params.expdate = "";
                            // if (hppAgree3.selected || hppAgree4.selected) {
                            //     params.cardAgree = "E";
                            // } else {
                            //     params.cardAgree = hppAgree.selectedValue;
                            // }
                        } else {
                            params.cardno = "";
                            params.cardtype = "";
                            params.expdate = "";
                            params.cardAgree = "";
                        }
                        /*********************선택상품검증 ************************************/
                        var acStr = "";
                        for (var k = 0; k < this.acDG.length; k++) {
                            
                            var item = this.acDG[k];
                            console.log("여기");
                            // 기존소스코드
                            // acStr = acStr +item.s_v1 + "^";
                            // stemp = item.s_v2;
                            // stemp = stemp.replace(/,/g, "");
                            // acStr = acStr + stemp + "^";
                            // stemp = item.s_v3;
                            // stemp = stemp.replace(/,/g, "");
                            // acStr = acStr + stemp + "^";
                            // stemp = item.s_v4;
                            // stemp = stemp.replace(/,/g, "");
                            // acStr = acStr + stemp + "^";
                            // stemp = item.s_v5;
                            // stemp = stemp.replace(/,/g, "");
                            // acStr = acStr + stemp + "^";
                            // stemp = item.s_v6;
                            // stemp = stemp.replace(/,/g, "");
                            // ihap = ihap + Number(stemp)    //결제합계
                            // acStr = acStr + stemp + "^";
                            // acStr = acStr + item.s_v7 + "^";
                            // acStr = acStr + item.s_v8 + "^";
                            // acStr = acStr + item.s_v9 + "^";
                            // stemp = item.s_v2;
                            // stemp = stemp.replace(/,/g, "");
                            // isu = isu + Number(stemp);  //총수량
                            acStr = acStr + item.v_nam + "^";
                            stemp = item.count;
                            acStr = acStr + stemp + "^";
                            stemp = item.tot_price;
                            acStr = acStr + stemp + "^";
                            stemp = item.tot_price;
                            acStr = acStr + stemp + "^";
                            stemp = 0;
                            acStr = acStr + stemp + "^";
                            stemp = item.tot_price;
                            ihap = ihap + Number(stemp)    //결제합계
                            acStr = acStr + stemp + "^";
                            acStr = acStr + "" + "^";
                            acStr = acStr + item.v_cod + "^";
                            acStr = acStr + item.rt_scod + "^";
                            stemp = item.count;
                            isu = isu + Number(stemp);  //총수량

                        }

                        params.acStr = acStr;
                        params.mcact = mcact;
                        params.ordergbn = ordergbn;   // 즉시승인 20  수정	30 승인취소 40 추가 50						
                        if (this.COMCODE.substr(0, 1) == "I" || this.COMCODE.substr(0, 1) == "C") {        // 결제 및 현금영수증 발행 회사 선택
                            params.cfrmno = "INICIS";
                        } else if (this.COMCODE.substr(0, 1) == "K" || this.COMCODE.substr(0, 1) == "G") {
                            params.cfrmno = "KCP";
                        } else if (this.COMCODE.substr(0, 1) == "A" || this.COMCODE.substr(0, 1) == "B" || this.COMCODE.substr(0, 1) == "F") {
                            params.cfrmno = "ALLAT";
                        } else if (this.COMCODE.substr(0, 1) == "N" || this.COMCODE.substr(0, 1) == "E") {
                            params.cfrmno = "NICE";
                        } else if (this.COMCODE.substr(0, 1) == "D" || this.COMCODE.substr(0, 1) == "H") {
                            params.cfrmno = "DANAL";
                        }
                        params.cfrmtype = this.strcfrmtype;	        //카드결제 -현금영수증 결제 확인

                        /****************** 결제금액정보 *************************************************/
                        params.runcarcod = Number(ihap / isu);               // 기본단가     
                        params.appno = isu;                  // 수량
                        params.runmoney = this.span_totmoney;			// 상품총금액 (기본단가 * 수량 )
                        params.span_surtax = this.surtax;      //부가세
                        params.span_tax = this.tax;            //과세
                        params.span_taxfree = this.taxfree;    //비과세
                        params.span_totmoney = this.runmoney;   //총결제금액(세금포함)(부가세별로 일때는 상품총액보다 10%많다)
                        params.span_sale = this.span_sale;           //할인금액
                        params.Taxselect = this.taxTypeSelect // 과세구분
                        /*******************************************************************/

                        /****************** 상품정보 *************************************************/
                        params.ocont = this.ocont;       // 상품 특이사항

                        if (k < 1) {
                            params.spointnm = this.acDG[0].v_nam;
                            this.GSANGNAME = this.acDG[0].v_nam;
                        } else {
                            params.spointnm = this.acDG[0].v_nam + "(" + k + "건)";
                            this.GSANGNAME = this.acDG[0].v_nam + "(" + k + "건)";
                        }
                        params.appnm = "0/0/0/0";   //상품그룹정보
                        /*******************************************************************/

                        /****************** SMS 관련 *************************************************/
                        params.hppcorp = this.GSMSYN;  //결제 SMS발송체크
                        // 선택적으로 사용*****************************************
                        params.apptel = this.urlEmail + "||" + this.urlTel;    //SMS인증일때 사용
                        /****************************************************************************/

                        let self = this;
                        let urls = Vue.prototype.$mainUrl + '/gate/__MDULES/FLEX21/reception_transact_V10.asp';
                        console.log(params);

                        $.ajax({
                            url: urls,
                            dataType: 'text',
                            type: 'post',
                            data: params,
                            error: function (jqXHR, textStatus, errorThrown) { // tranSend_faultHandler
                                self.showAlert("[fl1010][388][승인대기]데이타전송중 또는 해당 웹페이지 오류. 다시 시도 바랍니다.");
                                self.checkUser();
                            },
                            success: function (res) {
                                console.log(res);
                                // try {
                                let json = JSON.parse(res.replace(/\r/gi, '\\n').replace(/\n/gi, ' '));
                                self.tranSend_resultHandler(json);
                                // } catch (error) {
                                //     self.$swal({
                                //         html: "<span class='alert_txt'>JSON 파싱 에러</span>",
                                //         showCloseButton: false,
                                //         showCancelButton: false,
                                //         confirmButtonText: '확인',
                                //         focusConfirm: false,
                                //         allowOutsideClick: false
                                //     });
                                // }
                            }
                        });

                    } else {
                        this.btVisible("TV");
                    }
                }
            },
            tranSend_resultHandler(json) {
                var strprice = "";
                var strtaxfree = "";
                var strsurtax = "";
                var strtax = "";
                var scardno = "";
                var sdates = "";
                var params = new Object();
                var paramsStr = "";
                var tbuyertel = "";
                var tbuyeremail = "";
                var tbuyername = "";
                var url = "";

                if (json["sessionok"].session == "Y") {
                    if (this.tfast_join == "2") {	// 즉시결제 일때					
                        if (this.strcfrmtype == "0103" || this.strcfrmtype == "0104") {

                            tbuyertel = this.custtel;
                            tbuyeremail = this.custemail;
                            tbuyername = this.custnm;

                            if (tbuyertel == "") {
                                tbuyertel = "00000000000";
                            }
                            if (tbuyeremail == "") {
                                tbuyeremail = "00@00.net";
                            }
                            if (tbuyername == "") {
                                tbuyername = "미등록고객";
                            }

                            params.mcact = "TR05"
                            params.fast_join = 2
                            params.buyertel = tbuyertel;
                            params.buyeremail = tbuyeremail;
                            params.oid = this.orderno;
                            params.buyername = tbuyername;
                            params.price = this.runmoney;
                            params.taxfree = this.taxfree;
                            params.surtax = this.surtax;
                            params.tax = this.tax;
                            params.runmoney = this.span_totmoney;
                            params.span_surtax = this.surtax;
                            params.span_tax = this.tax;
                            params.span_taxfree = this.taxfree;
                            params.span_totmoney = this.runmoney;
                            params.sname = this.GSANGNAME;

                            if (this.strcfrmtype == "0103" || this.strcfrmtype == "0102") {
                                scardno = this.cardno1 + this.cardno2 + this.cardno3 + this.cardno4;
                                params.popcard = this.POPCARD;
                                params.cardno = scardno;
                                params.expdateMM = this.expdate1;
                                params.expdateYY = this.expdate2;
                                params.hppcorp = this.GSMSYN;
                                params.halbu = this.HALBU;
                                if (this.COMCODE.substr(0, 1) == "I") {
                                    url = "/gate/__MDULES/FLEX21/INSafeKeyINver2.asp"
                                } else if (this.COMCODE.substr(0, 1) == "C") {
                                    url = "/gate/__MDULES/FLEX21/INSafeKeyINver2_give.asp"
                                } else if (this.COMCODE.substr(0, 1) == "K") {
                                    url = "/gate/__MDULES/FLEX21/KCP_Card_ok.asp"
                                } else if (this.COMCODE.substr(0, 1) == "G") {
                                    url = "/gate/__MDULES/FLEX21/KCP_Card_ok_give.asp"
                                } else if (this.COMCODE.substr(0, 1) == "A") {
                                    url = "/gate/__MDULES/FLEX21/ALLAT_Card_ok.asp"
                                } else if (this.COMCODE.substr(0, 1) == "B" || this.COMCODE.substr(0, 1) == "F") {
                                    url = "/gate/__MDULES/FLEX21/ALLAT_Card_ok_give.asp"
                                } else if (this.COMCODE.substr(0, 1) == "E") {
                                    url = "/gate/__MDULES/FLEX21/Nice_Card_ok_give.asp";
                                } else if (this.COMCODE.substr(0, 1) == "N") {
                                    url = "/gate/__MDULES/FLEX21/Nice_Card_ok.asp";
                                } else if (this.COMCODE.substr(0, 1) == "D") {
                                    url = "/gate/__MDULES/FLEX21/DANAL_CARD_OK.asp"
                                } else if (this.COMCODE.substr(0, 1) == "H") {
                                    url = "/gate/__MDULES/FLEX21/DANAL_CARD_OK_give.asp"
                                }
                                //httpINS.url ="/gate/__MDULES/FLEX21/INSafeKeyINver2.asp";
                                //httpINS.url ="/gate/__MDULES/FLEX21/Nice_Card_ok.asp";
                            } else if (this.strcfrmtype == "0104") {
                                params.orderno = this.orderno;
                                if (this.receiptTypeRadio == "9400") {
                                    params.ptype =  this.receiptTypeRadio;
                                    url ="/gate/__MDULES/FLEX21/Pay_INIPay_V10.asp";	
                                } else {

                                
                                params.payme = this.ARR_PAYME;
                                params.ptype = this.receiptTypeRadio;
                                params.pnum = this.PAYMV1;


                                if (this.COMCODE.substr(0, 1) == "I") {
                                    url = "/gate/__MDULES/FLEX21/INSafeKeyINver2_pay.asp"
                                } else if (this.COMCODE.substr(0, 1) == "C") {
                                    url = "/gate/__MDULES/FLEX21/INSafeKeyINver2_pay_give.asp"
                                } else if (this.COMCODE.substr(0, 1) == "K") {
                                    url = "/gate/__MDULES/FLEX21/KCP_Pay_ok.asp"
                                } else if (this.COMCODE.substr(0, 1) == "G") {
                                    url = "/gate/__MDULES/FLEX21/KCP_Pay_ok_give.asp"
                                } else if (this.COMCODE.substr(0, 1) == "A") {
                                    url = "/gate/__MDULES/FLEX21/ALLAT_Pay_ok.asp"
                                } else if (this.COMCODE.substr(0, 1) == "B" || this.COMCODE.substr(0, 1) == "F") {
                                    url = "/gate/__MDULES/FLEX21/ALLAT_Pay_ok_give.asp"
                                } else if (this.COMCODE.substr(0, 1) == "E") {
                                    url = "/gate/__MDULES/FLEX21/Nice_Pay_ok_give.asp";
                                } else if (this.COMCODE.substr(0, 1) == "N") {
                                    url = "/gate/__MDULES/FLEX21/Nice_Pay_ok.asp";
                                } else if (this.COMCODE.substr(0, 1) == "D") {
                                    //httpINS.url ="/gate/__MDULES/FLEX21/DANAL_PAY_OK.asp";
                                } else if (this.COMCODE.substr(0, 1) == "H") {
                                    //httpINS.url ="/gate/__MDULES/FLEX21/DANAL_PAY_OK_give.asp";
                                }
                            }
                                        }
                            this.tranSend1(url, params);

                        } else {
                            params.mcact = "TR0201";
                            params.ordernos = this.orderno + "@";
                            url = "/gate/__MDULES/FLEX21/INIars_batch.asp";
                            this.tranSend2(url, params);
                        }
                    } else if (this.tfast_join == "4") {  //수정일때
                        this.showAlert("수정이 완료 되었습니다.");
                        this.checkUser();
                    } else {     // 승인대기 일때
                        if (this.GURLSMSVAL == "Y") {
                            this.Certify('URL_SMS');
                        }
                        this.checkUser();
                    }
                } else {
                    this.showAlert("주문저장 에러<br>[에러]입력한 정보가 잘 못 되었습니다. 수정 후 다시 시도해 주세요.");
                    if (this.CTEMP == "ORDER") {   //접수된 도더를 클릭했을 때	
                        this.btVisible("TV");
                    } else {
                        this.bottn1 = true;
                        // this.bottn6 = true; B06 ??
                        this.bottn2 = true;
                    }
                    // LOAD1.visible = false;
                    // LOAD2.visible = false;
                }
            },
            tranSend1(url, params) {

                let urls = Vue.prototype.$mainUrl + url;
                console.log(urls);
                var self = this;
                $.ajax({
                    url: urls,
                    dataType: 'text',
                    type: 'post',
                    data: params,
                    error: function (jqXHR, textStatus, errorThrown) { // INSSend_faultHandler
                        self.showAlert("[fl1010][388][결제진행중오류]데이타전송중 또는 해당 웹페이지 오류. 다시 시도 바랍니다.");
                        self.btVisible("TV");
                    },
                    success: function (res) { // INSSend_resultHandler
                        console.log(res);
                        try {
                            let json = JSON.parse(res.replace(/\r/gi, '\\n').replace(/\n/gi, ' '));

                            var tmp1 = "";
                            var tmp2 = "";
                            var tmp3 = "";


                            if (self.strcfrmtype == "0104") {
                                tmp1 = "현금 결제 성공";
                                tmp2 = "현금 결제 실패";
                                tmp3 = "현금 결제가 성공 했습니다.";
                            } else {
                                tmp1 = "카드 결제 성공";
                                tmp2 = "카드 결제 실패";
                                tmp3 = "카드 결제가  성공 했습니다.";
                            }
                            if (json["ResultOk"].ResultCode == "00") {
                                self.showAlert(tmp1 + "<br>성공 : " + tmp3);
                            } else if (json["ResultOk"].ResultCode == "02") {
                                self.showAlert("문자발송에러<br>[결제성공][알림실패] : 결제는 성공했으나 결제내역을 문자로 전송중 에러가 발생 했습니다.");
                            } else {
                                self.showAlert("실패 : " + json["ResultOk"].ResultMsg);
                            }

                            self.checkUser();

                        } catch (error) {
                            self.showAlert("JSON 파싱 에러");
                            self.payingState = false;
                        }
                    }
                });
            },
            tranSend2(url, params) {
                let urls = Vue.prototype.$mainUrl + url;
                var self = this;
                $.ajax({
                    url: urls,
                    dataType: 'text',
                    type: 'post',
                    data: params,
                    error: function (jqXHR, textStatus, errorThrown) { // faultHandler1
                        self.showAlert("오류발생");
                        self.btVisible("TV");
                    },
                    success: function (res) { // resultHandler1
                        console.log(res);
                        try {
                            let json = JSON.parse(res.replace(/\r/gi, '\\n').replace(/\n/gi, ' '));
                            self.showAlert("승인요청 되었습니다.");
                            self.checkUser();
                        } catch (error) {
                            self.showAlert("JSON 파싱 에러");
                            self.payingState = false;
                        }
                    }
                });

            },
            chkUrlSEND() {
                var tstr = true;
                if (this.U02 == "SMS") {   //SMS
                    if (this.urlTel == "") {   // 전화번호
                        this.$refs.urlTel.focus()
                        this.showAlert("전화번호를 입력해 주세요");
                        tstr = false;
                    }
                } else if (U01.selected) { //이메일
                    if (urlEmail.text == "") {   // 이메일
                        this.$refs.urlTel.focus()
                        this.showAlert("이메일을 입력해 주세요");
                        tstr = false;
                    }
                } else {
                    this.showAlert("전화번호를 입력해 주세요");
                    tstr = false;
                }

                return tstr;
            },
            sendData() {
                var params = new Object();
                var sTmp = this.srchWord;
                params.orderno = this.orderno;
                console.log("orderno : " + this.orderno);
                params.srchField = this.srchField;
                if (this.srchField == "070101" || this.srchField == "ALL") {
                    params.srchWord = sTmp.replace(/-/g, "");
                } else {
                    params.srchWord = sTmp;
                }

                if (this.srchWord != "" || this.orderno != "") {
                    this.doCardRequest(params);
                } else {
                    this.payingState = false;
                }
            },
            doCardRequest(params) {
                console.log(params);
                let self = this;
                let urls = Vue.prototype.$mainUrl + '/gate/__MDULES/FLEX21/F_xml_writeform_V10.asp';

                $.ajax({
                    url: urls,
                    dataType: 'text',
                    type: 'post',
                    data: params,
                    error: function (jqXHR, textStatus, errorThrown) { // cardRequest_faultHandler
                        self.showAlert("[fl1010][232][전화번호조회]데이타전송중 또는 해당 웹페이지 오류. 다시 시도 바랍니다.");
                        self.payingState = false;
                    },
                    success: function (res) {                        
                        console.log(res);
                        //# try {
                        let json = JSON.parse(res.replace(/\r/gi, '\\n').replace(/\n/gi, ' '));
                        if (json["resultCode"] == "200") {
                            var sessionok = json["sessionok"];
                            if (sessionok.session == "Y") {
                                self.doCardRequestOK(json);
                            } else {
                                self.showAlert("세션만료");
                                self.payingState = false;
                            }
                        } else {
                            self.showAlert("실패 : " + json["resultCode"]);
                            self.payingState = false;
                        }
                        // } catch (error) {
                        //     self.$swal({
                        //         html: "<span class='alert_txt'>JSON 파싱 에러</span>",
                        //         showCloseButton: false,
                        //         showCancelButton: false,
                        //         confirmButtonText: '확인',
                        //         focusConfirm: false,
                        //         allowOutsideClick: false
                        //     });
                        // }
                    }
                });
            },
            checkUser() {
                this.payingState = false;
                // var params1 = new Object();

                // if (this.orderno != "") {
                //     params1.orderno = this.orderno;
                //     params1.mcact = "close";

                //     let self = this;
                //     let urls = Vue.prototype.$mainUrl + '/gate/__MDULES/FLEX21/INISafeRe.asp';

                //     $.ajax({ // chkUser
                //         url: urls,
                //         dataType: 'text',
                //         type: 'post',
                //         data: params1,
                //         error: function (jqXHR, textStatus, errorThrown) { // chkUser_faultHandler
                            // self.$swal({
                            //     html: "<span class='alert_txt'>[fl1010][1325][닫기버튼 클릭시 중복사용자 체크 해제 에러] 데이타전송중 또는 해당 웹페이지 오류. 다시 시도 바랍니다.</span>",
                            //     showCloseButton: false,
                            //     showCancelButton: false,
                            //     confirmButtonText: '확인',
                            //     focusConfirm: false,
                            //     allowOutsideClick: false
                            // });
                //             self.closePopup(); // 임시로 닫기 이벤트 걸어둠
                //         },
                //         success: function (res) { // chkUser_resultHandler
                //             console.log(res);
                //             self.closePopup(); // self.closed('N');
                //         }
                //     });
                // } else {   // 접수창을 열기만 하고 닫을 때
                    this.closePopup(); // closed1(), closed('Y/N')
                // }
            },
            closePopup() {
                this.itemList = [];
                this.filteredList = [];
                this.acDG = []; // 선택된 상품 목록(우측에 표기)
                this.groupList = []; // 대분류 리스트 
                this.groupBtn = []; // 대분류 리스트 중 화면에 표기할 5개 항목 리스트                
                this.groupClicked = "전체메뉴";
                this.srchField0 = "0401";
                this.srchField = "070101";
                this.srchWord = "";
                this.camove = [];
                this.caLog = [];
                this.orderno = "";
                this.searchStr = ""; // 상품검색키워드
                this.ARR_PAYME = "핸드폰번호";
                this.PAYMV1 = "";
                this.halbus = [];
                this.HALBU = "00";
                this.CTEMP = "NOCUST";   //오더구분   =  신규팝업오더  = NOCUST  신규팝업+고객검색 :CUST  오더클릭 :ORDER
                this.CARD_COM = "";
                this.BCUM01 = false;
                this.viewType = "grid";
                this.custname = "";
                this.inputTypeRadio = "메뉴그룹사용";
                this.inputTypeRadio2 = "카드결제";
                this.strcfrmtype = "0103";
                this.receiptTypeRadio = "9400";
                this.receiptTypeRadio2 = "";
                this.U02 = "SMS";
                this.urlTel = "";
                this.RSELECT = "N";
                this.RSELECTID = "0";
                this.receiptCardType = false;
                this.botbtn1 = false; // B01
                this.botbtn2 = false; // B02
                this.botbtn3 = false; // B03
                this.botbtn4 = false; // B04
                this.botbtn5 = false;
                this.span_totmoney = 0;
                this.discountPrice = 0;
                this.span_sale = "0";
                this.payPrice = 0;
                this.runmoney = "0";
                this.taxTypeSelect = "9103";
                this.selfView = false;
                this.ocont = "";
                this.tax = 0; // 과세
                this.surtax = 0; // 부가세
                this.taxfree = 0; // 면세
                this.tax_span = '0';
                this.surtax_span = '0';
                this.taxfree_span = '0';
                this.selfItemName = "";
                this.selfCount = 1;
                this.selfCount_span = "1";
                this.selfUnitPrice = 0;
                this.selfUnitPrice_span = "0";
                this.selfPrice = 0;
                this.selfPrice_span = "0";
                this.tcustno = "";
                this.tcustnm = "";
                this.tcusttel = "";
                this.strtmp = "";
                this.tcont = "";
                this.custnm = "";
                this.custtel = "";
                this.custType = "";
                this.cfcompno = "";
                this.custemail = "";
                this.tcustemail = "";
                this.custid = "";
                this.custno = "";
                this.CUSTCHECK = "N";
                this.COMCODE = "NNNNNNNNNN";
                this.COMCODE01 = "N";
                this.GSMSYN = "Y";
                this.GSVAL = "0@0@0@0@0";
                this.orderstate = "";
                this.strVianm = "";
                this.urlTel = "";
                this.urlEmail = "";
                this.recEmail = "";
                this.recTel = "";
                this.cardno1 = "";
                this.cardno2 = "";
                this.cardno3 = "";
                this.cardno4 = "";
                this.expdate1 = "";
                this.expdate2 = "";
                this.tid = "";
                this.ordermsg0 = "";
                this.ordermsg = "";
                this.lcnclcont = "";
                this.GURLSMSVAL = "N"; // 결제창 SMS ; 이메일 전송
                this.addr = "";
                this.tfast_join = "";
                this.GSANGNAME = "";
                this.POPCARD = "N";
                this.custtelenabled = false;
                this.custView = true;
                this.itemView = true;
                this.listView = true;
                this.buttonView = true;
                this.payResendView = false;
                this.payCheckView = true;
                this.cancelView = true;
                this.srchOk = false;
                this.taxfreeInput = false;
                this.payingState = false;
                this.rg6 = "A";
                this.GPCSID = "";
                this.partial = false;
                this.$emit('close');
                this.$modal.hide('order-popup3');
                this.memo = ""; //메모
                this.unpublish = true;
            },
            make2Digit(number) {
                if (number < 10) {
                    return "0" + number;
                } else {
                    return number;
                }
            },
            doCardRequestOK(json) { // cardRequest_resultHandler     
                console.log(json);                          
                this.payingState = false;
                var checkuser = json["checkuser"];
                var strtemp = checkuser.cfrmno;
                var strCnt = checkuser.custCnt;
                var strCntNo = checkuser.custCntNo;
                var self = this;

                if (strCnt == "POPUPOK") {	  //오더가   1개이상 이거나 없을 때 팝업 뛰운다
                    if (strCntNo == "0") {
                        this.srchWord = "";
                        this.showAlert("검색한 데이타가 없습니다. 다시 시도해 주세요.");
                    } else {
                        this.searchPOP();
                    }
                } else {	//오더가   1개일때

                    var ComCode = json["ComCode"];
                    this.COMCODE = ComCode.codnm1;
                    this.COMCODE01 = ComCode.codnm2;

                    this.RSELECT = "N";
                    this.RSELECTID = "0";

                    var cuinfo = null;
                    if(Array.isArray(json["cuinfo"])) {
                        cuinfo = (json["cuinfo"])[0];
                    } else {
                        cuinfo = json["cuinfo"];
                    }
                    if (strtemp == "N") {  // 이  오더를  클릭한 상담원이 없다면...
                        this.BCUM01 = true;   //고객상세내역 선택가능하게

                        /***************************************고객정보 관련 ********************************/
                        
                        var strtel = cuinfo.custtel;
                        var strtmp = "";

                        this.tcustno = cuinfo.custno;
                        this.tcustnm = cuinfo.custnm;
                        this.tcusttel = strtel;
                        this.strtmp = cuinfo.billkey;
                        this.tcont = cuinfo.cont;
                        this.custnm = cuinfo.custnm;
                        
                        this.custtel = strtel;

                        // f_ims.text = cuinfo.f_ims;
                        this.memo = cuinfo.f_imm;  //메모
                        this.addr = cuinfo.f_ims;

                        if (cuinfo.f_ome == "0402") {   //사업자 고객
                            this.custType = "R002";
                            this.visibleSet("0402");
                            // tf_cpo.text = cuinfo.f_cpo;
                            // tf_one.text = cuinfo.f_one;
                            // tf_obo.text = cuinfo.f_obo;
                            // tf_ode.text = cuinfo.f_ode;
                            // tf_omp.text = cuinfo.f_omp;
                            // tf_oea.text = cuinfo.f_oea;
                            this.custtelenabled = true;
                            this.cfcompno = cuinfo.f_cpo;
                        } else {  // 일반고객
                            this.custType = "R001";
                            this.visibleSet("0401");
                            this.cfcompno = "해당없음";
                            this.custtelenabled = false;
                        }

                        if (cuinfo.custemail == "") {
                            this.custemail = "oo@oo.net";
                            this.tcustemail = "oo@oo.net";
                        } else {
                            this.custemail = cuinfo.custemail;
                            this.tcustemail = cuinfo.custemail;
                        }

                        this.custid = cuinfo.custid;
                        this.custno = cuinfo.custno;
                        
                        if (this.orderno == "" || this.orderno == undefined) {
                            this.orderno = cuinfo.orderno;
                        }

                        var camoveData = json["camove"];
                        this.camove = [];

                        
                        if (Object.prototype.toString.call(camoveData).slice(8, -1) == "Array") {
                            for (var i = 0; i < camoveData.length; i++) {
                                var item = camoveData[i];
                                this.camove.push({
                                    orderdate: item.orderdate,
                                    orderstatecodnm: item.orderstatecodnm,
                                    spointnm: item.spointnm,
                                    epointnm: item.epointnm,
                                    totmoney: item.totmoney,
                                    cont: item.cont,
                                    cfrmtype: item.cfrmtype,
                                    apptel: item.apptel,
                                    appnm: item.appnm
                                })
                            }
                        }
                        var calogData = json["caLog"];
                        this.caLog = [];
                        if (Object.prototype.toString.call(calogData).slice(8, -1) == "Array") {
                            for (var i = 0; i < calogData.length; i++) {
                                var item = calogData[i];
                                this.caLog.push({
                                    gubun: item.gubun,
                                    contents: item.contents,
                                    rcsid: item.rcsid,
                                    rcsnm: item.rcsnm,
                                    wdate: item.wdate,
                                    totmoney: item.totmoney,
                                    runmoney: item.runmoney,
                                    surtax: item.surtax,
                                    tax: item.tax,
                                    taxfree: item.taxfree,
                                    spointnm: item.spointnm,
                                    epointnm: item.epointnm,
                                    runcarcodnm: item.runcarcodnm,
                                    drivernm: item.drivernm,
                                    apptel: item.apptel,
                                    appnm: item.appnm
                                })
                            }
                        }

                        this.COMCODE = json["ComCode"].codnm1.substr(0, 1); //# NN으로 나와서 N으로 바꾸기 위해 한자리만 substr
                        this.COMCODE01 = json["ComCode"].codnm2;

                        // txtID01.visible = false;
                        if (cuinfo.f_ome == "0402") {
                            this.$refs.custtel.focus();
                        } else {
                            this.$refs.custnm.focus();
                        }

                        /***************************************고객정보 관련 끝********************************/

                        this.GSMSYN = cuinfo.hppcorp;
                        /******************************오더일때 (리스트에서 클릭해서 )******************************************************************/
                        if (this.CTEMP == "ORDER") {
                            var cuOrder = (json["cuOrder"])[0];
                            this.GSVAL = cuOrder.appnm;
                            this.ocont = cuOrder.cont;
                            this.strcfrmtype = cuOrder.cfrmtype;  		  //카드/현금 구분 : 0103,0104 // CFRMTYPEVAL
                            this.orderstate = cuOrder.orderstate;   //오더상태 : 0201 

                            this.strVianm = cuOrder.vianm;   //영수증 값 
                            /***************검색창 화면 컨트롤****************************/
                            // srchWord.enabled = false;
                            // B11.enabled = false;
                            /***************결제수단 선택****************************/
                            if (this.strcfrmtype == "0103"||this.strcfrmtype == "0102") {
                                /**************************결제 창전송 관련 SMS/이메일 ******************/
                                var strApptel = cuOrder.apptel;   //접수창전송
                                if (strApptel != "") {
                                    var index = strApptel.indexOf("||");
                                    if (index != -1) {
                                        // urlEmail.text = fncSplit(strApptel, 0, "||");  //결제창전송 이���일						
                                        this.urlTel = strApptel.split("||")[1];    //결제창전송 전화번호
                                        // if (this.urlTel != "") {
                                        //     if (urlTel.text != "0000000000") {
                                        //         U02.selected = true;
                                        //     } else {
                                        //         urlTel.text = "";
                                        //     }
                                        // } else if (String(urlEmail.text) != "") {
                                        //     U01.selected = true;
                                        // }
                                    } else { //||구분자가 없을 때
                                        if (strApptel != "0000000000") {
                                            // U02.selected = true;
                                            // urlTel.text = strApptel;    //결제창전송 전화번호
                                        }
                                    }
                                } else {
                                    this.urlTel = "";
                                }
                                /**********************************************************************/
                            }
                            //# 임의추가
                            this.recEmail = this.custemail;
                            this.recTel = this.custtel;
                            /***************오더상태에 따른 화면 제어****************************/
                            this.viewSetting();
                            if (this.orderstate != "0201") {

                                /**************결제버튼 처리/영수증표기*********************/
                                if (this.strcfrmtype == "0103" || this.strcfrmtype == "0102") {
                                    switch (this.orderstate) {
                                        case "0202":
                                            this.enabledDataGroup(false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false);
                                            this.enabledCustomerUrl(false, false, false, false, false, false);  //url창 
                                            this.enabledButton(false, false, false, true, false); //우측버튼  표기  대기,결제,수정,취소,전송
                                            this.enabledReceiptCard(false, false, false, false, false, false);  //카드영수증창 선택안되게
                                            break;
                                        case "0203":
                                            this.enabledDataGroup(false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false);
                                            this.enabledCustomerUrl(false, false, false, false, false, false);  //url창 
                                            this.enabledButton(false, false, false, true, false);
                                            this.enabledReceiptCard(false, false, false, false, false, false);  //카드영수증창 선택안되게
                                            break;
                                        case "0204":
                                            this.enabledDataGroup(false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false);
                                            this.enabledCustomerUrl(false, false, false, false, false, false);  //url창 
                                            this.enabledButton(false, false, false, false, false);
                                            this.enabledReceiptCard(false, false, false, false, false, false);  //카드영수증창 선택안되게
                                            break;
                                        case "0205":
                                            this.enabledDataGroup(false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false);
                                            this.enabledCustomerUrl(false, false, false, false, false, false);  //url창 
                                            this.enabledButton(false, false, false, false, false);
                                            this.enabledReceiptCard(false, false, false, false, false, false);  //카드영수증창 선택안되게
                                            break;
                                        case "0206": // 결제완료
                                            this.enabledDataGroup(false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false);
                                            this.enabledCustomerUrl(false, false, false, false, false, false);  //url창 
                                            this.enabledButton(false, false, false, true, false);
                                            this.enabledReceiptCard(true, true, true, true, true, true);  //카드영수증창 선택안되게
                                            this.recEmail = this.strVianm.split("||")[0];  //영수증 이메일
                                            this.recTel = this.strVianm.split("||")[1];    //영수증 전화번호
                                            this.recTel = this.custtel; //# 임의작업
                                            this.partial = true;
                                            break;
                                        case "0207":
                                            this.enabledDataGroup(false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false);
                                            this.enabledCustomerUrl(false, false, false, false, false, false);  //url창 
                                            this.enabledButton(false, false, false, false, false);
                                            this.enabledReceiptCard(true, true, true, true, true, true);  //카드영수증창 선택안되게
                                            this.recEmail = this.strVianm.split("||")[0];  //영수증 이메일
                                            this.recTel = this.strVianm.split("||")[1];    //영수증 전화번호
                                            break;
                                        default:
                                            this.enabledDataGroup(false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false);
                                            this.enabledReceiptCard(false, false, false, false, false, false);  //카드영수증창 선택안되게
                                            this.enabledButton(false, true, true, true, false);
                                    }
                                } else if (this.strcfrmtype == "0104") {
                                    this.enabledDataGroup(false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false);
                                    this.enabledCustomerUrl(false, false, false, false, false, false);
                                    switch (this.orderstate) {
                                        case "0202":
                                            this.enabledButton(false, false, false, true, false); //우측버튼  표기  대기,결제,수정,취소,전송
                                            this.enabledReceiptPay(false, false, false, false, false, false);  //현금영수증창 선택안되게
                                            break;
                                        case "0203":
                                            this.enabledButton(false, false, false, true, false);
                                            this.enabledReceiptPay(false, false, false, false, false, false);  //현금영수증창 선택안되게
                                            break;
                                        case "0204":
                                            this.enabledButton(false, false, false, false, false);
                                            this.enabledReceiptPay(false, false, false, false, false, false);  //현금영수증창 선택안되게
                                            break;
                                        case "0205":
                                            this.enabledButton(false, false, false, false, false);
                                            this.enabledReceiptPay(false, false, false, false, false, false);  //현금영수증창 선택안되게
                                            break;
                                        case "0206":
                                            this.enabledButton(false, false, false, true, false);
                                            this.enabledReceiptPay(true, true, true, true, true, true);  //현금영수증창 선택안되게
                                            this.recTel = this.strVianm.split("||")[0];  //영수증 이메일 PrecTel
                                            this.recEmail = this.strVianm.split("||")[1];    //영수증 전화번호 PrecEmail
                                            // PAYME.visible = false;
                                            // PAYMV1.visible = false;
                                            break;
                                        case "0207":
                                            this.enabledButton(false, false, false, false, false);
                                            this.enabledReceiptPay(true, true, true, true, true, true);  //현금영수증창 선택안되게
                                            this.recTel = this.strVianm.split("||")[0];  //영수증 이메일 PrecTel
                                            this.recEmail = this.strVianm.split("||")[1];    //영수증 전화번호 PrecEmail
                                            // PAYME.visible = false;
                                            // PAYMV1.visible = false;
                                            break;
                                        default:
                                            this.enabledReceiptPay(false, false, false, false, false, false);  //현금영수증창 선택안되게
                                            this.enabledButton(false, true, true, true, false);
                                    }
                                }

                            } else {  //승인대기일때
                                if (this.strcfrmtype == "0103" || this.strcfrmtype == "0102") {
                                    this.enabledButton(false, true, true, true, false);
                                    this.enabledReceiptCard(false, false, false, false, false, false);  //카드영수증창 선택안되게
                                } else if (this.strcfrmtype == "0104") {
                                    this.enabledCustomerUrl(false, false, false, false, false, false);
                                    this.enabledButton(false, true, true, true, false);
                                    this.enabledReceiptPay(false, false, false, false, false, false);  //카드영수증창 선택안되게
                                }
                            }

                            /************************************상품내역 리스트 추가*****************************************************/
                            var resultArray = json["ordersub"];
                            this.acDG = [];
                            for (var j = 0; j < resultArray.length; j++) {
                                var obj = {
                                    rt_pay: Number(resultArray[j].s_v3) / Number(resultArray[j].s_v2),
                                    rt_reg1: "",
                                    rt_reg2: "",
                                    rt_reg3: "",
                                    rt_scod: resultArray[j].s_v9,
                                    v_cod: resultArray[j].s_v8,
                                    v_img: "",
                                    v_nam: resultArray[j].s_v1,
                                    clicked: true,
                                    count: resultArray[j].s_v2,
                                    tot_price: resultArray[j].s_v4,
                                    rt_pay_span: this.makeComma(Number(resultArray[j].s_v3) / Number(resultArray[j].s_v2)),
                                    count_span: this.makeComma(resultArray[j].s_v2),
                                    tot_price_span: this.makeComma(resultArray[j].s_v4)
                                };
                                // 기존코드
                                // obj["s_v1"] = resultArray[j].s_v1;
                                // obj["s_v3"] = this.makeComma(resultArray[j].s_v3);
                                // obj["s_v4"] = this.makeComma(resultArray[j].s_v4);
                                // obj["s_v5"] = resultArray[j].s_v5;
                                // obj["s_v6"] = this.makeComma(resultArray[j].s_v6);
                                // obj["s_v7"] = resultArray[j].s_v7;
                                // obj["s_v8"] = resultArray[j].s_v8;
                                // obj["s_v9"] = resultArray[j].s_v9;

                                this.acDG.push(obj);
                                for (var i = 0; i < this.filteredList.length; i++) {
                                    if (obj.v_cod == this.filteredList[i].v_cod) {
                                        this.filteredList[i].clicked = true;
                                    }
                                }
                                this.changMenuBtn("SELECT", "btn01", resultArray[j].s_v9, 0, 0, 0, 0, 0);
                            }

                            // Refresh Data
                            this.gridOptions.api.redrawRows();

                            /************************************상품내역 리스트 추가 끝*****************************************************/

                            /******************************상품 금액 정보******************************************************************/
                            
                            
                            // this.runmoney = this.makeComma(cuOrder.runmoney[0]);  //상품총액					
                            // this.taxfree = cuOrder.taxfree[0];
                            // this.tax = cuOrder.tax[0];
                            // this.surtax = cuOrder.surtax[0];
                            // this.span_totmoney = this.makeComma(cuOrder.totmoney[0]);

                            var cuOrder = (json["cuOrder"])[0];
                            this.discountPrice = Number(cuOrder.sale);
                            this.taxTypeSelect = cuOrder.epointnm;
                            this.tax = cuOrder.tax;
                            this.taxfree = cuOrder.taxfree;
                            this.surtax = cuOrder.surtax;

                            this.span_totmoney = this.makeComma(Number(cuOrder.runmoney));
                            this.payPrice = this.makeComma(Number(cuOrder.totmoney));
                            this.span_sale = this.makeComma(this.discountPrice);
                            this.tax_span = this.makeComma(this.tax);
                            this.surtax_span = this.makeComma(this.surtax);
                            this.taxfree_span = this.makeComma(this.taxfree);
                            this.runmoney = this.makeComma(Number(cuOrder.totmoney));
                            //this.calcPrice(); 210125_금액계산안하는것으로수정

                            /******************************카드결제창 및 현금영수증창 표시 정보******************************************************************/
                            if (this.strcfrmtype == "0101" || this.strcfrmtype == "0102" || this.strcfrmtype == "0103") {
                                var strcasenum1 = cuOrder.CASENUM1;
                                var strcasenum2 = cuOrder.CASENUM2;
                                this.cardno1 = strcasenum1.substr(0, 4);
                                this.cardno2 = strcasenum1.substr(4, 4);
                                this.cardno3 = strcasenum1.substr(8, 4);
                                this.cardno4 = strcasenum1.substr(12, strcasenum1.length - 12);
                                this.expdate1 = strcasenum2.substr(0, 2);
                                this.expdate2 = strcasenum2.substr(3, 2);

                                this.HALBU = cuOrder.driverno;   //할부						
                                this.tid = cuOrder.tid.replace(/ /g, "");
                                this.ordermsg0 = "[" + cuOrder.orderstatenm + "] " + "승인번호:" + cuOrder.authcode;
                                this.ordermsg = "내용 : " + cuOrder.resultmsg;
                                //     this.tid = dataset[0].cuOrder.tid[0];
                                this.lcnclcont = cuOrder.cnclcont;
                                this.CARD_COM = cuOrder.cardtype; // 카드사

                            } else if (this.strcfrmtype == "0104") {
                                this.receiptTypeRadio2 = cuOrder.drivertel;
                                //영수증버튼
                                this.orderstate = cuOrder.orderstate;
                                //TID
                                this.tid = cuOrder.tid.replace(/ /g, "");
                               

                                if ( cuOrder.authcode == "") {
                                     //상태
                                    this.ordermsg0 = "[" + cuOrder.orderstatenm + "] " + cuOrder.resultmsg;
                                } else {
                                    this.ordermsg0 = "[" + cuOrder.orderstatenm + "] " + " 승인번호:" + cuOrder.authcode + 
                                    " "+ cuOrder.resultmsg;
                                }

                                //취소내역
                                this.lcnclcont = cuOrder.cnclcont;

                            } else if (this.strcfrmtype == "0102" || this.strcfrmtype == "0103") {
                                this.tid = cuOrder.tid.replace(/ /g, "");
                                this.ordermsg0 = "[" + cuOrder.orderstatenm + "] " + "승인번호:" + cuOrder.authcode;
                                this.ordermsg = "내용 : " + cuOrder.resultmsg;
                                //     this.tid = dataset[0].cuOrder.tid[0];
                                this.lcnclcont = cuOrder.cnclcont;
                            }
                        } else {	// 주문오더가  아닐때
                            this.CTEMP = "CUST";
                            this.srchOk = true;
                            // custnm.enabled = true;     //고객명
                            // addr.enabled = true;
                            // custemail.enabled = true;   //고객이메일
                            this.orderstate = "0201";   // 오더 상태
                            // urlS1.enabled = false;
                            // CTIT.enabled = false;
                            // U02.enabled = false;
                            // urlTel.enabled = false;

                            // U02.selected = true; //SMS 결제창 전송
                            this.urlTel = this.custtel;
                            // urlEmail.text = "" //결제창전송 이메일

                        }
                        
                        /*********오더일때 끝**************************************************************************************************/
                    } else {
                        if (this.CUSTCHECK == "Y") {
                            this.CUSTCHECK = "N"
                        } else {
                            self.showAlert("[" + strtemp + "] 상담원이 접속해 작업을 하고 있습니다.<br>작업이 끝난 후 다시 시도해 주세요.");
                            self.closePopup(); // self.closed('Y');
                        }
                    }

                    if (cuinfo.f_ome == "") {
                        this.CHK_SEARCH();
                    }
                }
            },
            viewSetting() {
                switch (this.orderstate) {
                    case "0201": // 승인대기
                        this.custView = false;
                        this.itemView = true;
                        this.payCheckView = true;
                        this.listView = true;
                        this.buttonView = true;    
                        this.payResendView = true;
                        this.cancelView = true;
                        break;
                    case "0202":// 승인전취소
                        this.custView = false;
                        this.itemView = false;
                        this.payCheckView = false;
                        this.listView = false;
                        this.buttonView = false;
                        this.payResendView = false;
                        this.cancelView = false;
                        break;
                    case "0203":// 승인요청중
                        this.custView = false;
                        this.itemView = false;
                        this.payCheckView = false;
                        this.listView = false;
                        this.buttonView = false;
                        this.payResendView = false;
                        this.cancelView = true;
                        break;
                    case "0204":// 승인요청중취소
                        this.custView = false;
                        this.itemView = false;
                        this.payCheckView = false;
                        this.listView = false;
                        this.buttonView = false;
                        this.payResendView = false;
                        this.cancelView = true;
                        break;
                    case "0205":// 승인결과취소
                        this.custView = false;
                        this.itemView = false;
                        this.payCheckView = false;
                        this.listView = false;
                        this.buttonView = false;
                        this.payResendView = false;
                        this.cancelView = true;
                        break;
                    case "0206":// 승인완료
                        this.custView = false;
                        this.itemView = false;
                        this.payCheckView = false;
                        this.listView = false;
                        this.buttonView = false;
                        this.payResendView = false;
                        this.cancelView = true;
                        break;
                    case "0207":// 승인후취소
                        this.custView = false;
                        this.itemView = false;
                        this.payCheckView = false;
                        this.listView = false;
                        this.buttonView = false;
                        this.payResendView = false;
                        this.cancelView = true;
                        break;
                    default:
                        break;
                }
            },
            /*************************그룹메뉴창 화면처리 시작 ***********************/
            enabledDataGroup(t1, t2, t3, t4, t5, t6, t7, t8, t9, t10, t11, t12, t13, t14, t15, t16, t17, t18) {
                // srchSangpum.enabled = t1; 
                // B0.enabled = t2; 
                // GP1.enabled = t3; 
                // GP2.enabled = t4; 
                // btn_previous.enabled = t5; 
                // btn_next.enabled = t6; 
                // horizontalList.enabled = t7; 
                // btnEnabled = t8;   //그룹메뉴버튼  비활성화
                // dataGroup1.enabled = t9; 						
                // Taxselect.enabled = t10;//과세방식 선택						
                // grtype1.enabled = t11; //결제방식 선택
                // grtype2.enabled = t12;					   
                // BTN01.enabled = t13;  //상품 수량 변경 버튼 
                // BTN02.enabled = t14; 
                // BTN03.enabled = t15; 
                // BTN04.enabled = t16; 
                // BTN05.enabled = t17; 
                // BTN06.enabled = t18; 			
            },
            /*************************고객 직접결제  화면처리 시작 ***********************/
            enabledCustomerUrl(t1, t2, t3, t4, t5, t6) {
                // U01.enabled = t1;
                // U02.enabled = t2;
                // urlTel.enabled = t3;
                // urlEmail.enabled = t4;
                // urlS1.enabled = t5;
                // CTIT.enabled = t6;				
            },
            CHK_SEARCH() {
                // Alert.okLabel="일반고객";
                // Alert.cancelLabel ="사업자";				
                // Alert.show("일반고객 또는 사업자 고객을 선택해 주세요.", "고객유형선택",Alert.OK|Alert.CANCEL, this, chk_handle, null);	
            },
            searchPOP() {
                var str01 = this.srchField0;
                var str02 = this.srchField;
                var str03 = this.srchWord;
                this.$modal.show('order-custselect', { str01: str01, str02: str02, str03: str03 }); // POP_reception_Group
            },
            selectPOP() {
                // var str01:String = srchField0.selectedItem.data;
                // var str02:String = srchField.selectedItem.data;
                // var str03:String = srchWord.text;
                // FlexGlobals.topLevelApplication.TOPPOPIMG = "alls.png";
                // var mp:POP_reception_select = POP_reception_select(PopUpManager.createPopUp(this,POP_reception_select,true));

                // mp.openner = this;				
                // mp.init1();		
                // mp.x = int((this.width - mp.width)/2) + 300;
                // mp.y = int((this.height - mp.height)/2)+ 150;
            },
            visibleSet(t1) {
                if (t1 == "0402") {
                //     BC01.height = 181;
                //     tf_one.visible = true;
                //     tf_obo.visible = true;
                //     tf_ode.visible = true;
                //     tf_omp.visible = true;
                //     tf_oea.visible = true;
                //     tf_cpo.visible = true;
                //     txt01.visible = true;
                //     txt02.visible = true;
                //     Tx01.visible = true;
                //     Tx02.visible = true;
                //     Tx03.visible = true;
                //     Tx04.visible = true;
                //     Tx05.visible = true;
                //     Tx06.visible = true;

                //     TX001.text = "회사명";
                //     TX002.text = "회사이메일"
                } else {
                //     BC01.height = 85;
                //     tf_one.visible = false;
                //     tf_obo.visible = false;
                //     tf_ode.visible = false;
                //     tf_omp.visible = false;
                //     tf_oea.visible = false;
                //     tf_cpo.visible = false;
                //     txt01.visible = true;
                //     txt02.visible = true;
                //     custemail.visible = true;
                //     Tx01.visible = false;
                //     Tx02.visible = false;
                //     Tx03.visible = false;
                //     Tx04.visible = false;
                //     Tx05.visible = false;
                //     Tx06.visible = false;
                //     TX001.text = "고객명";
                //     TX002.text = "이메일"
                }
            },
            getByteB(str) {
                var byte = 0;
                for (var i=0; i<str.length; ++i) {
                // 기본 한글 2바이트 처리
                (str.charCodeAt(i) > 127) ? byte += 2 : byte++ ;
                }
                return byte;
            },
            sendData1()   //[조회] 및 전화번호 창에서 엔터 칠때 동작
            {
                var params = new Object();
                var self = this;
                if (this.RSELECT == "Y") {
                    params.scid = this.RSELECTID;
                    this.doCardRequest(params);
                } else {
                    var temp = this.srchWord;
                    if (this.srchWord == "") {
                        self.showAlert("전화번호 또는 고객번호를 입력해 주세요.");
                        self.payingState = false;
                        return;
                    }
                    if (this.srchField == "070101" && this.getByteB(temp) > 15) {
                        self.showAlert("전화번호는 숫자로 15자까지만 가능합니다. 확인 후 다시 입력해 주세요.");
                        self.payingState = false;
                        return;
                    }

                    params.srchField0 = this.srchField0;
                    params.srchField = this.srchField;
                    if (this.srchField == "070101" || this.srchField == "ALL") {
                        params.srchWord = temp.replace(/-/g, "");
                    } else {
                        params.srchWord = temp;
                    }

                    if (this.RSELECT == "NN") {	//중복고객 원할 때
                        params.srchField = "NEW_OVERLAP";
                    }

                    if (this.srchWord != "") {
                        this.doCardRequest(params);
                    } else {
                        this.payingState = false;
                    }
                }
            },
            showAlert(msg) {
                this.$swal({
                    html: "<span class='alert_txt'>"+msg+"</span>",
                    showCloseButton: false,
                    showCancelButton: false,
                    confirmButtonText: '확인',
                    focusConfirm: false,
                    allowOutsideClick: false
                });
            },
            getData: function () { // 상품 목록 조회
                let self = this;
                self.groupList.push("전체메뉴");
                for (var i = 0; i < Vue.prototype.$Price_list.length; i++) {
                    var obj = Vue.prototype.$Price_list[i];
                    var item = {
                        rt_pay: self.removeComma(obj.rt_pay),
                        rt_reg1: obj.rt_reg1,
                        rt_reg2: obj.rt_reg2,
                        rt_reg3: obj.rt_reg3,
                        rt_scod: obj.rt_scod,
                        v_cod: obj.v_cod,
                        v_img: obj.v_img,
                        v_nam: obj.v_nam,
                        tot_price: 0,
                        count: 0,
                        clicked: false
                    };
                    self.itemList.push(item);
                    self.filteredList.push(item);
                    self.groupList.push(obj.rt_reg1);
                }

                // 대분류 중복제거 및 그룹 버튼 리스트 생성
                self.groupList = self.groupList.reduce(function (a, b) {if (a.indexOf(b) < 0) a.push(b); return a;}, []);
                self.groupStartPos = 0;
                self.updateGroupBtnList();
                // 대분류 중 전체메뉴 버튼 클릭
                self.clickGroupBtn("전체메뉴");
            },
            updateGroupBtnList: function () {
                this.groupBtn = [];
                var self = this;
                for (var i = this.groupStartPos; i < this.groupStartPos + this.groupBtnLimit; i++) {
                    if (self.groupList[i] == self.groupClicked) {
                        this.groupBtn.push({
                            title: self.groupList[i],
                            clicked: true
                        });
                    } else {
                        this.groupBtn.push({
                            title: self.groupList[i],
                            clicked: false
                        });
                    }
                }
                // for (var i = this.groupStartPos; i < this.groupList.length; i++) {
                //     if (self.groupList[i] == self.groupClicked) {
                //         this.groupBtn.push({
                //             title: self.groupList[i],
                //             clicked: true
                //         });
                //     } else {
                //         this.groupBtn.push({
                //             title: self.groupList[i],
                //             clicked: false
                //         });
                //     }
                // }
            },
            clickGroupBtn: function (title) {
                this.groupClicked = title;
                for (var i = 0; i < this.groupBtn.length; i++) {
                    if (this.groupBtn[i].title == title) {
                        this.groupBtn[i].clicked = true;
                    }
                }
                this.filteredList = [];
                for (var i = 0; i < this.itemList.length; i++) {
                    var item = this.itemList[i];
                    if (title == '전체메뉴') {
                        this.filteredList.push(item);
                    } else {
                        if (item.rt_reg1 == title) {//대그룹명
                            this.filteredList.push(item);
                        }
                    }

                }
                this.updateGroupBtnList();
            },
            groupNavi: function (type) {
                if (type == "prev") {
                    if (this.groupStartPos != 0) {
                        this.groupStartPos = this.groupStartPos - 1;
                    }
                } else if (type == "next") {
                    if (this.groupStartPos != this.groupList.length - this.groupBtnLimit) {
                        this.groupStartPos = this.groupStartPos + 1;
                    }
                }
                this.updateGroupBtnList();
            },
            gridClick: function (position) { // 좌측의 그리드 선택 이벤트
                this.filteredList[position].clicked = true;
                var obj = this.filteredList[position];

                var item = {
                    rt_pay: obj.rt_pay,
                    rt_reg1: obj.rt_reg1,
                    rt_reg2: obj.rt_reg2,
                    rt_reg3: obj.rt_reg3,
                    rt_scod: obj.rt_scod,
                    v_cod: obj.v_cod,
                    v_img: obj.v_img,
                    v_nam: obj.v_nam,
                    clicked: false,
                    count: 1,
                    tot_price: obj.rt_pay,
                    rt_pay_span: this.makeComma(obj.rt_pay),
                    count_span: "1",
                    tot_price_span: this.makeComma(obj.rt_pay)
                };

                var self = this;
                var isNewItem = true;
                for (var i = 0; i < this.acDG.length; i++) {
                    if (self.acDG[i].v_cod == item.v_cod) {
                        self.acDG[i].count = (Number)(self.acDG[i].count) + 1;
                        self.acDG[i].tot_price = (Number)(self.acDG[i].count) * (Number)(self.acDG[i].rt_pay);

                        self.acDG[i].count_span = self.makeComma(self.acDG[i].count);
                        self.acDG[i].tot_price_span = self.makeComma(self.acDG[i].tot_price);

                        isNewItem = false;
                    }
                }

                if (isNewItem) { //신규등록이면 리스트에 push
                    this.acDG.push(item);
                }
                this.gridOptions.api.redrawRows(); // ag-grid 데이터 업데이트해주기
                this.calcPrice();
            },
            makeComma: function (str) { // 숫자에 콤마 표기
                str = String(str);
                return str.replace(/(\d)(?=(?:\d{3})+(?!\d))/g, '$1,');
            },
            removeComma: function (str) { // 콤마 표기 제거
                return str.replace(/,/gi, "");
            },
            calcPrice: function () { // taxMove와 합칠것
                // 1. 구매금액, 결제금액 계산
                this.span_totmoney = 0;
                for (var i = 0; i < this.acDG.length; i++) {
                    this.span_totmoney = this.span_totmoney + Number(this.acDG[i].rt_pay) * Number(this.acDG[i].count);
                }

                this.payPrice = Number(this.span_totmoney) - Number(this.discountPrice);
                this.span_sale = this.makeComma(this.discountPrice);

                // 2. 과세방식에 따라 과세, 부가세, 면세 금액 계산
                this.calcTax();
            },
            calcTax: function () {
                switch (this.taxTypeSelect) {
                    case "9103": // 부가세포함
                        this.tax = Math.round(Number(this.payPrice) / 1.1);
                        this.surtax = Math.round(Number(this.payPrice) - Number(this.tax))
                        this.taxfree = 0;
                        break;
                    case "9102": // 부가세별도
                        this.tax = this.payPrice;
                        this.surtax = Math.round(Number(this.tax) * 0.1);
                        this.taxfree = 0;
                        break;
                    case "9105": // 비부세포함
                        this.tax = Math.round((Number(this.payPrice) - Number(this.taxfree)) / 1.1);
                        this.surtax = Number(this.payPrice) - Number(this.taxfree) - Number(this.tax);
                        break;
                    case "9101": // 비부세별도
                        this.tax = Number(this.payPrice) - Number(this.taxfree);
                        this.surtax = Math.round(Number(this.tax) * 0.1);
                        break;
                    case "9106": // 면세
                        this.tax = 0;
                        this.surtax = 0;
                        this.taxfree = this.payPrice;
                        break;
                }
                this.tax_span = this.makeComma(this.tax);
                this.surtax_span = this.makeComma(this.surtax);
                this.taxfree_span = this.makeComma(this.taxfree);
                console.log(this.makeComma(this.taxfree));
                this.runmoney = this.makeComma(Number(this.tax)+Number(this.surtax)+Number(this.taxfree));
            },
            findItem: function () {
                this.filteredList = [];
                for (var i = 0; i < this.itemList.length; i++) {
                    var item = this.itemList[i];
                    if (item.v_nam.includes(this.searchStr)) {
                        this.filteredList.push(item);
                    }
                }
            },
            selfAdd: function () {
                if (this.selfItemName == "" || this.selfCount == 0) {
                    this.showAlert("상품명 혹은 수량을 정확히 입력해주세요.");
                    return;
                }

                var item = {
                    rt_pay: this.selfUnitPrice,
                    rt_reg1: "",
                    rt_reg2: "",
                    rt_reg3: "",
                    rt_scod: "",
                    v_cod: "",
                    v_img: "",
                    v_nam: this.selfItemName,
                    clicked: false,
                    count: Number(this.selfCount),
                    tot_price: this.selfPrice,
                    rt_pay_span: this.makeComma(this.selfUnitPrice),
                    count_span: this.makeComma(this.selfCount),
                    tot_price_span: this.makeComma(this.selfPrice)
                };

                this.acDG.push(item);
                this.calcPrice();
            },
            delList() {
                var self = this;
                var removeList = this.gridOptions.api.getSelectedRows();
                //
                for (var i = 0; i < this.itemList.length; i++) {
                    for (var j = 0; j < removeList.length; j++) {
                        var item = self.itemList[i];
                        var r = removeList[j];
                        if (item.v_nam == r.v_nam && item.v_cod == r.v_cod) {
                            self.itemList[i].clicked = false;

                        }
                    }
                } 

                for (var i = 0; i < this.acDG.length; i++) {
                    for (var j = 0; j < removeList.length; j++) {
                        var s = self.acDG[i];
                        var r = removeList[j];
                        if (s.v_nam == r.v_nam && s.v_cod == r.v_cod) {
                            self.acDG.splice(i, 1);
                        }
                    }
                }

                for (var i = 0; i < this.filteredList.length; i++) {
                    for (var j = 0; j < removeList.length; j++) {
                        var obj = self.filteredList[i];
                        var r = removeList[j];
                        if (obj.v_nam == r.v_nam && obj.v_cod == r.v_cod) {
                            self.filteredList[i].clicked = false;

                        }
                    }
                }
                this.gridOptions.api.redrawRows(); // ag-grid 데이터 업데이트해주기
                this.calcPrice();
            },
            updateCount: function (type) {
                var updateList = this.gridOptions.api.getSelectedRows();
                let self = this;

                for (var i = 0; i < this.acDG.length; i++) {
                    for (var j = 0; j < updateList.length; j++) {
                        var s = self.acDG[i];
                        var u = updateList[j];
                        if (s.v_nam == u.v_nam && s.v_cod == u.v_cod) {
                            if (type == "+") {
                                self.acDG[i].count = self.acDG[i].count + 1;
                            // } else if (type == "-" && s.count == 1) {
                            //     for (var k = 0; k < self.filteredList.length; k++) {
                            //         var obj = self.filteredList[k];
                            //         if (obj.v_nam == u.v_nam && obj.v_cod == u.v_cod) {
                            //             self.filteredList[k].clicked = false;
                            //         }
                            //     }
                            //     self.acDG.splice(i, 1);
                            } else if (type == '-' && s.count != 1) {
                                self.acDG[i].count = self.acDG[i].count - 1;
                            }
                            self.acDG[i].count_span = self.makeComma(self.acDG[i].count);
                            self.acDG[i].tot_price = self.acDG[i].count * self.acDG[i].rt_pay;
                            self.acDG[i].tot_price_span = self.makeComma(self.acDG[i].tot_price);
                            
                        }
                    }
                }
                this.gridOptions.api.redrawRows(); // ag-grid 데이터 업데이트해주기
                this.calcPrice();
            },
            ButtonCheck(mval) {                
               if (this.payButtonCheck == "Y") {
                    if (Vue.prototype.$authcod == "1") {
                            this.strcfrmtype = '0104';
                     } else {
                           this.$swal({
                                 html: "<span class='alert_txt'>현금영수증 결제 권한이 없습니다. 관리자만 현금영수증 결제가 가능합니다.</span>",
                                 showCloseButton: false,
                                 showCancelButton: false,
                                 confirmButtonText: '확인',
                                 focusConfirm: false,
                                 allowOutsideClick: false
                             });
                     } 
               } else {
                   this.strcfrmtype = '0104'; 
               }      

            },
            Certify(mmode) {			
                var params = new Object();
                var url = "";

                if (this.strcfrmtype == '0104') {
                    mmode = "REC_SMS_PAY";
                }
                switch( mmode ){					
                    // case "HPP_SMS":	
                        
                    //     params.mmode = "HPP_SMS";	
                    //     params.RANDOMNUM = this.RandomNum();	
                    //     params.TELNO = this.custtel ;		
                    //     httpCode.url ="/gate/__MDULES/flex/SMS_Certify.asp";							
                    //     break;	
                    case "URL_SMS":	
                        // if (String(rg4.selectedValue) == "SMS"){
                            params.mmode = "URL_SMS";						
                            params.TELNO = this.urlTel ;		
                            params.ORDERNO = this.orderno;
                            url ="/gate/__MDULES/flex21/SMS_Certify.asp";	
                        // }else {
                        //     params.mmode = "URL_EMAIL";						
                        //     params.TELNO = this.urlEmail ;		
                        //     params.ORDERNO = this.orderno;
                        //     httpCode.url ="/gate/__MDULES/flex/EMAIL_Certify.asp";
                        // }
                        break;	
                    case "REC_SMS":	 // 카드영수증 문자/이메일
                        if (this.U02 == "SMS"){
                            params.mmode = "REC_SMS";						
                            params.TELNO = this.recTel;
                            params.ORDERNO = this.orderno;
                            params.TID = this.tid;
                            params.CCODE = this.COMCODE;		//I이니시스  N:나이스						
                            url ="/gate/__MDULES/flex21/SMS_Certify.asp";	
                        }else {
                            mmode = "REC_EMAIL";
                            params.mmode = "REC_EMAIL";						
                            params.TELNO = this.recEmail;		
                            params.ORDERNO = this.orderno;
                            params.TID = this.tid;
                            params.CCODE = this.COMCODE;		//I이니시스  N:나이스	
                            params.OSTATE = this.orderstate;		//상태, 결제완료 취소
                            url ="/gate/__MDULES/flex21/EMAIL_Certify.asp";							
                        }
                        break;	
                    case "REC_SMS_PAY":	

                           if (this.U02 == "SMS"){
                            mmode = "REC_SMS_PAY";
                            params.mmode = "REC_SMS_PAY";						
                            params.TELNO = this.recTel;
                            params.ORDERNO = this.orderno;
                            params.TID = this.tid;
                            params.CCODE = this.COMCODE;		//I이니시스  N:나이스						
                            url ="/gate/__MDULES/flex21/SMS_Certify.asp";	
                        }else {
                            mmode = "REC_EMAIL_PAY";
                            params.mmode = "REC_EMAIL_PAY";						
                            params.TELNO = this.recEmail;		
                            params.ORDERNO = this.orderno;
                            params.TID = this.tid;
                            params.CCODE = this.COMCODE;		//I이니시스  N:나이스	
                            params.OSTATE = this.orderstate;		//상태, 결제완료 취소
                            url ="/gate/__MDULES/flex21/EMAIL_Certify.asp";							
                        }




                        // if (String(rg3.selectedValue) == "SMS"){
                        //     params.mmode = "REC_SMS_PAY";						
                        //     params.TELNO = PrecTel.text ;		
                        //     params.ORDERNO = orderno.text;
                        //     params.TID = payTid.text;
                        //     params.CCODE = this.COMCODE;		//I이니시스  N:나이스						
                        //     httpCode.url ="/gate/__MDULES/flex/SMS_Certify.asp";	
                        // }else {
                        //     params.mmode = "REC_EMAIL_PAY";						
                        //     params.TELNO = PrecEmail.text ;		
                        //     params.ORDERNO = orderno.text;
                        //     params.TID = payTid.text;
                        //     params.CCODE = this.COMCODE;		//I이니시스  N:나이스	
                        //     params.OSTATE = orderstate.text;		//상태, 결제완료 취소
                        //     httpCode.url ="/gate/__MDULES/flex/EMAIL_Certify.asp";							
                        // }
                        break;
                    
                }
                
                let self = this;
                let urls = Vue.prototype.$mainUrl + url;
                //console.log(params);

                $.ajax({
                    url: urls,
                    dataType: 'text',
                    type: 'post',
                    data: params,
                    error: function (jqXHR, textStatus, errorThrown) { // Certify_faultHandler
                        self.showAlert("[fl1010][980][전송에러]데이타전송중 또는 해당 웹페이지 오류. 다시 시도 바랍니다.");
                        self.checkUser();
                    },
                    success: function (res) { // Certify_resultHandler
                        console.log(res);
                        try {
                            let json = JSON.parse(res.replace(/\r/gi, '\\n').replace(/\n/gi, ' '));
                            var check = json["checkok"].check;
                            var popStr = "";
                            
                            switch( mmode ){
                                // case "HPP_SMS":							
                                //         if (check == "T") {	 
                                //             Alert.show("SMS 인증문자가 전송되었습니다.","인증문자전송성공");							
                                //         }else {
                                //             Alert.show("SMS 인증문자가 전송되지 않았습니다. 다시 시도해 주세요","인증문자전송실패");							
                                //         }			
                                //     break;
                                case "URL_SMS":							
                                    if (check == "T") {
                                        popStr = "결제문자전송성공<br>[성공]고객에게 결제 URL이 문자로 발송 되었습니다.";
                                    }else {
                                        popStr = "결제문자전송실패<br>[실패]고객에게 결제 URL이 발송되지 않았습니다. 다시 시도해 주세요.";
                                    }			
                                    break;	
                                // case "URL_EMAIL":							
                                //     if (check == "T") {	 
                                //         Alert.show("[성공]고객에게 결제 이메일이 발송 되었습니다." ,"결제이메일전송성공" );							
                                //     }else {
                                //         Alert.show("[실패]고겍에게 결제 이메일이 발송되지 않았습니다. 다시 시도해 주세요." + checkmsg ,"결제이메일전송실패" );							
                                //     }			
                                //     break;	
                                case "REC_SMS":							
                                    if (check == "T") {
                                        popStr = "카드영수증문자전송성공<br>[성공]카드영수증 문자가  발송 되었습니다";
                                    }else {
                                        popStr = "카드영수증문자실패<br>[실패]카드영수증 문자발송이 되지  않았습니다. 다시 시도해 주세요.";
                                    }			
                                    break;	
                                case "REC_EMAIL":							
                                    if (check == "T") {	 
                                        popStr = "카드영수증이메일전송성공<br>[성공]카드영수증이 이메일로  발송 되었습니다.";
                                    }else {
                                        popStr = "카드영수증이메일실패<br>[실패]카드영수증 이메일 발송이 실패했습니다. 다시 시도해 주세요." + json["checkok"].checkmsg;
                                    }			
                                    break;	
                               case "REC_SMS_PAY":							
                                    if (check == "T") {	 
                                        popStr = "[성공]현금영수증 문자가  발송 되었습니다.";							
                                    }else {
                                       popStr = "[실패]현금영수증 문자발송이 되지  않았습니다. 다시 시도해 주세요." + json["checkok"].checkmsg;							
                                    }			
                                    break;	
                                case "REC_EMAIL_PAY":							
                                    if (check == "T") {	 
                                       popStr = "[성공]현금영수증이 이메일로  발송 되었습니다.";							
                                    }else {
                                       popStr = "[실패]현금영수증 이메일 발송이 실패했습니다. 다시 시도해 주세요." + json["checkok"].checkmsg;						
                                    }			
                                    break;	
                            }

                            if(popStr != "") {
                                self.showAlert(popStr);
                            }
                            
                            
                        } catch (error) {
                            self.showAlert("JSON 파싱 에러");
                        }
                    }
                });
            },
            RandomNum(){
                var tstr = String(Math.random()*10000000000000);
                if (tstr.length > 5) {
                    return tstr.substr(0,6);
                }else{
                    tstr = tstr + "183650"
                    return tstr.substr(0,6);
                }
            },
            receiptpop(){
                var url = "";                
                if (this.strcfrmtype == "0103" || this.strcfrmtype == "0102") {                        
                    if (this.COMCODE.substr(0, 1) == "I") {
                        url = "https://iniweb.inicis.com/app/publication/apReceipt.jsp?noTid=" + this.tid + "&flag=0&noMethod=1&tStat=null&re_mail=Y";
                        window.open(url,'','width=420, height=840, toolbar=no, menubar=no,scrollbars=1');
                    }else if (this.COMCODE.substr(0, 1) == "K"){
                        
                    }else if (this.COMCODE.substr(0, 1) == "A"){
                        url = "http://www.allatpay.com/servlet/AllatBizPop/member/pop_card_receipt.jsp?shop_id=" + Vue.prototype.$default["shopinfo"].aid  +"&order_no=" + this.orderno + "&flag=0&noMethod=1&tStat=null&re_mail=Y"
                        window.open(url,'','width=410, height=650, toolbar=no, menubar=no,scrollbars=1');
                    }else if (this.COMCODE.substr(0, 1) == "B"){  //올엣 지급대행 분할정산
                        url = "https://giveapi.tele-pay.kr/api/recin/allat/v1/" + this.tid +"/" + this.orderno;
                        window.open(url,'','width=480, height=680, toolbar=no, menubar=no,scrollbars=no');
                    }else if (this.COMCODE.substr(0, 1) == "F"){  //올엣 지급대행 재정산
                        url = "https://giveapi.tele-pay.kr/api/recin/allat/v1/" + this.tid +"/" + orderno.text + "";
                        window.open(url,'','width=680, height=650, toolbar=no, menubar=no,scrollbars=1');
                        
                    }else if (this.COMCODE.substr(0, 1) == "D"){  //다날
                        url = Vue.prototype.$mainUrl + "/gate/__MDULES/flex/DANAL_CARD_RECEIPT.asp?tid=" + this.tid +"&pay=" + this.span_totmoney.replace(/,/g,"");
                        window.open(url,'','width=680, height=650, toolbar=no, menubar=no,scrollbars=1');
                                        
                    }else {
                        url = "https://npg.nicepay.co.kr/issue/CardIssue.do?TID=" + this.tid + "&svcCd=01&sendMail=1";
                        window.open(url,'','toolbar=no,location=no,directories=no,status=yes,menubar=no,scrollbars=yes,resizable=yes,width=420,height=700');
                    }
                }else if (this.strcfrmtype == "0104" ) {                    
                    if (this.COMCODE.substr(0, 1) == "I") {
                        url = "https://iniweb.inicis.com/DefaultWebApp/mall/cr/cm/Cash_mCmReceipt.jsp?noTid=" + this.tid + "&clpaymethod=22";
                        window.open(url,'','width=420, height=840, toolbar=no, menubar=no,scrollbars=1');
                    }else if (this.COMCODE.substr(0, 1) == "K") {
                    }else if (this.COMCODE.substr(0, 1) == "A" || this.COMCODE.substr(0, 1) == "B"){
                        url = "http://www.allatpay.com/servlet/AllatBizPop/member/pop_cash_receipt.jsp?receipt_seq_no=" + this.tid +"&shop_id=" + Vue.prototype.$default["shopinfo"].aid + "&amt=" + this.span_totmoney + "&flag=0&noMethod=1&tStat=null&re_mail=Y";
                        window.open(url,'','width=410, height=650, toolbar=no, menubar=no,scrollbars=1');
                    }else if (this.COMCODE.substr(0, 1) == "F"){
                        url = "https://giveapi.tele-pay.kr/api/repin/allat/v1/" +  this.tid +"/" + this.orderno;
                        window.open(url,'','width=410, height=650, toolbar=no, menubar=no,scrollbars=1');
                    }else {
                        url = "https://npg.nicepay.co.kr/issue/CashIssueCart.do?TID=" + this.tid + "&svcCd=null&sendMail=1";
                        window.open(url,'','toolbar=no,location=no,directories=no,status=yes,menubar=no,scrollbars=yes,resizable=yes,width=420,height=540');
                    }
                }
            },
            sendReceipt() {
            },
            setTextPattern(e) {
                let message = e.target.value
                let pattern = /[^0-9A-Za-zㄱ-ㅎ|ㅏ-ㅣ|가-힣\*\(\)\-\ \_\+\|\?\/\ ]/i
                this.valid = (message.length > 1 && pattern.test(message) === false)
            },
            taxToTaxfree() {
                var self = this;
                this.$swal({
                    html: "<span class='alert_txt'>[선택금액 비과세등록]<br>선택한 금액을 비과세로 하시겠습니까?</span>",
                    showCloseButton: false,
                    showCancelButton: true,
                    cancelButtonText: '닫기',
                    confirmButtonText: '확인',
                    focusConfirm: false,
                    showLoaderOnConfirm: true
                }).then((result) => {
                    if (result.value) {
                        var list = self.gridOptions.api.getSelectedRows();
                        var price = 0;
                        for(var i=0; i<list.length; i++) {
                            console.log("ttt : "+list[i].tot_price);
                            price += Number(self.removeComma(list[i].tot_price));
                        }
                        self.taxfree = price;
                        self.calcPrice();
                    }
                })
            },
            onlyNumber(evt) {
               evt = (evt) ? evt : window.event;
               console.log();
                var charCode = (evt.which) ? evt.which : evt.keyCode;
                if(evt.key == ".") {
                    evt.preventDefault();
                }
                if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode != 46) {
                    evt.preventDefault();
                } else {
                    return true;
                } 
            },
            cardNumCheckBtn: function() {
                if (this.cardCheck()) {
                    this.showAlert("[카드번호 정상]<br>카드번호가 정상 입니다.");
                }else{
                    this.showAlert("[카드번호 체크 에러]<br>카드번호가 잘 못 되었습니다. 다시 확인 바랍니다.");
                }
            },
            cardCheck: function() {
                var numSum=999; 
                var num1,num2,num3,num4,num5,num6,num7,num8,num9,num10,num11,num12,num13,num14,num15,num16;
                var cardNumber = this.cardno1 + this.cardno2 + this.cardno3 + this.cardno4;
                if (cardNumber.length == 16) {
                    num15 = this.cardCheckHap(Number(cardNumber.charAt(14)) * 2);
                    num14 = this.cardCheckHap(Number(cardNumber.charAt(13)) * 1);
                    num13 = this.cardCheckHap(Number(cardNumber.charAt(12)) * 2);
                    num12 = this.cardCheckHap(Number(cardNumber.charAt(11)) * 1);
                    num11 = this.cardCheckHap(Number(cardNumber.charAt(10)) * 2);
                    num10 = this.cardCheckHap(Number(cardNumber.charAt(9)) * 1);
                    num9 = this.cardCheckHap(Number(cardNumber.charAt(8)) * 2);
                    num8 = this.cardCheckHap(Number(cardNumber.charAt(7)) * 1);
                    num7 = this.cardCheckHap(Number(cardNumber.charAt(6)) * 2);
                    num6 = this.cardCheckHap(Number(cardNumber.charAt(5)) * 1);
                    num5 = this.cardCheckHap(Number(cardNumber.charAt(4)) * 2);
                    num4 = this.cardCheckHap(Number(cardNumber.charAt(3)) * 1);
                    num3 = this.cardCheckHap(Number(cardNumber.charAt(2)) * 2);
                    num2 = this.cardCheckHap(Number(cardNumber.charAt(1)) * 1);
                    num1 = this.cardCheckHap(Number(cardNumber.charAt(0)) * 2);
                    
                    num16 = Number(cardNumber.charAt(15));
                    numSum = num1+num2+num3+num4+num5+num6+num7+num8+num9+num10+num11+num12+num13+num14+ num15+ num16;
                    numSum = numSum % 10;	
                }else if (cardNumber.length == 15 ){
                    num14 = this.cardCheckHap(Number(cardNumber.charAt(13)) * 2);
                    num13 = this.cardCheckHap(Number(cardNumber.charAt(12)) * 1);
                    num12 = this.cardCheckHap(Number(cardNumber.charAt(11)) * 2);
                    num11 = this.cardCheckHap(Number(cardNumber.charAt(10)) * 1);
                    num10 = this.cardCheckHap(Number(cardNumber.charAt(9)) * 2);
                    num9 = this.cardCheckHap(Number(cardNumber.charAt(8)) * 1);
                    num8 = this.cardCheckHap(Number(cardNumber.charAt(7)) * 2);
                    num7 = this.cardCheckHap(Number(cardNumber.charAt(6)) * 1);
                    num6 = this.cardCheckHap(Number(cardNumber.charAt(5)) * 2);
                    num5 = this.cardCheckHap(Number(cardNumber.charAt(4)) * 1);
                    num4 = this.cardCheckHap(Number(cardNumber.charAt(3)) * 2);
                    num3 = this.cardCheckHap(Number(cardNumber.charAt(2)) * 1);
                    num2 = this.cardCheckHap(Number(cardNumber.charAt(1)) * 2);
                    num1 = this.cardCheckHap(Number(cardNumber.charAt(0)) * 1);
                    
                    num15 = Number(cardNumber.charAt(14));
                    numSum = num1+num2+num3+num4+num5+num6+num7+num8+num9+num10+num11+num12+num13+num14+ num15;
                    numSum = numSum % 10;	
                }else if (cardNumber.length == 14 ){					
                    num13 = this.cardCheckHap(Number(cardNumber.charAt(12)) * 2);
                    num12 = this.cardCheckHap(Number(cardNumber.charAt(11)) * 1);
                    num11 = this.cardCheckHap(Number(cardNumber.charAt(10)) * 2);
                    num10 = this.cardCheckHap(Number(cardNumber.charAt(9)) * 1);
                    num9 = this.cardCheckHap(Number(cardNumber.charAt(8)) * 2);
                    num8 = this.cardCheckHap(Number(cardNumber.charAt(7)) * 1);
                    num7 = this.cardCheckHap(Number(cardNumber.charAt(6)) * 2);
                    num6 = this.cardCheckHap(Number(cardNumber.charAt(5)) * 1);
                    num5 = this.cardCheckHap(Number(cardNumber.charAt(4)) * 2);
                    num4 = this.cardCheckHap(Number(cardNumber.charAt(3)) * 1);
                    num3 = this.cardCheckHap(Number(cardNumber.charAt(2)) * 2);
                    num2 = this.cardCheckHap(Number(cardNumber.charAt(1)) * 1);
                    num1 = this.cardCheckHap(Number(cardNumber.charAt(0)) * 2);
                    
                    num14 = Number(cardNumber.charAt(13));
                    numSum = num1+num2+num3+num4+num5+num6+num7+num8+num9+num10+num11+num12+num13+num14;
                    numSum = numSum % 10;	
                    
                }else {
                    return false;					
                }
                
                if (numSum == 0) {
                    return true;
                }else{					
                    return false;
                }	
            },
            cardCheckHap: function(num) {
                if (num > 9) {
                    num = num - 10 + 1;	
                }				
                return num;
            },
        },
        beforeMount() {
            this.gridOptions = {
                defaultColDef: {
                    enableColResize: true,
                    enableSorting: true,
                    enableFilter: true,
                    animateRows: true,
                    resizable: true,
                    sortable: true
                },
                onRowDoubleClicked: this.doSomething
            }
            this.gridOptions2 = {
                defaultColDef: {
                    enableColResize: true,
                    enableSorting: true,
                    enableFilter: true,
                    animateRows: true,
                    resizable: true,
                    sortable: true
                },
            }
            this.gridOptions3 = {
                defaultColDef: {
                    enableColResize: true,
                    enableSorting: true,
                    enableFilter: true,
                    animateRows: true,
                    resizable: true,
                    sortable: true
                },
            }
            this.rowSelection = 'multiple';
        },
        
    }
</script>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR&display=swap');
  @font-face {
  font-family: 'Noto Sans KR';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url(https://fonts.gstatic.com/s/notosanskr/v27/PbykFmXiEBPT4ITbgNA5Cgm20xz64px_1hVWr0wuPNGmlQNMEfD4.5.woff2) format('woff2');
  unicode-range: U+d507, U+d509-d50b, U+d50d-d513, U+d515-d53b, U+d53e-d53f, U+d541-d543, U+d545-d54c, U+d54e, U+d550, U+d552-d557, U+d55a-d55b, U+d55d-d55f, U+d561-d564, U+d566-d567, U+d56a, U+d56c, U+d56e-d573, U+d576-d577, U+d579-d583, U+d585-d586, U+d58a-d5a4, U+d5a6-d5bb;
}
    * {
        box-sizing: border-box;
        font-family: 'Noto Sans KR';
    }
    body {
        
        font-family: 'Noto Sans KR';
    
    }
    button {
        border: none;
    }

    .order_popup_content {
        padding: 25px 25px 10px 25px;
        
    }
    /* header */
    .order_header {
        justify-content: space-between;      
        display: flex;
        align-items: center;  
        font-size: 24px;
        margin-bottom: 20px;
        margin-right: 0px;
        
    }
    .custom_select {
        height:25px; 
        padding: 2px 5px; 
        width: 90px;
        margin-right: 5px;
   }
    .custom_info {
        margin:5px;
    }
/* 내용 부분 */
    .order_content_wrap {
        padding: 10px;
        /* height: 600px; */
        width: 1035px;
    }

    .order_content_left {
        margin-right: 10px;
    }

    .order_content_right {
        /* flex: 1; */
    }

    .order_content_right a button {
        background-color: #006bc9;
        color: #ffffff;
        /* border: none; */
        /* height: 75px; */
        /* width: 85px; */
        /* border-radius: 5px; */
    }
    
    .input_label {
        padding: 3px;
        margin: 6px 5px 0px 15px; 
    }
    .input_label input {
        margin-right: 4px;
        margin-left: 5px;
        margin-top:1.5px;
       
    }
    .input_label label {
        margin-right:15px;
        display:flex; 
        float:left;
    }
    .input_label label:hover {
        cursor: pointer;
        color: rgb(0, 107, 201) !important;
    }
    .label_color {
        font-size:12px;
        color: #393c3f !important;

    }
    .custom_input {
        border: 1px solid #eeeeee !important;
        border-radius: 5px;
        height: 25px;
        color: #393c3f;
        font-weight: normal !important;
        
    }
 

  
    /* .item_group_wrapper {
        display: flex;
        text-align: center;
        margin-bottom: 10px;
        height: 30px;
    }

    .item_group_wrapper div {
        margin: 0px 10px;
        padding: 0px;
        width: 100%;
        height: 100%;
        flex: 1;
        display: flex;
    } */

    /* 상품 메뉴바 */
   
    .group_btn_on {
        cursor: pointer;
        color: white;
        /* font-weight: 400 !important; */
        background: #006bc9;
        /* width:90px !important; */
        border-radius: 5px;
        /* padding: 0 25px !important; */
        filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#7e7d7d', endColorstr='#616161', GradientType=0);
    }

    /* .group_btn_on:hover {
        background: #5a5b5b;
        background: -moz-linear-gradient(top, #5a5b5b 0%, #4b4b4c 50%, #3e3f40 51%, #393839 100%);
        background: -webkit-linear-gradient(top, #5a5b5b 0%, #4b4b4c 50%, #3e3f40 51%, #393839 100%);
        background: linear-gradient(to bottom, #5a5b5b 0%, #4b4b4c 50%, #3e3f40 51%, #393839 100%);
        filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#5a5b5b', endColorstr='#393839', GradientType=0);
    } */
    .group_btn_off {
        cursor: pointer;
        border-radius: 5px;
        background: #eff0f2;
    /* border: 1px solid rgba(57, 60, 63, 0.15); */
    color: #393C3F;
        opacity: 0.6;
    }

    .group_btn_off:hover {
        background: #D0E9FF !important;
        color: #006BC9 !important;
        filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#edeeef', endColorstr='#c9cbcc', GradientType=0) !important;
    }
    /* 리스트 상품 박스 */
    .grid {
        /* padding-right: 15px; */
    }
    .grid_item {
        border-radius: 5px;
        width: 115px !important;
        height: 95px;
        margin: 0px 5px 5px 0px;
        display: inline-block;
        padding: 10px;
        word-break: normal;
        font-size: 13px;
        cursor: pointer;
    }

    .grid_item:hover {
        background: rgba(0, 107, 201, 0.04) !important;
    }
    
    .grid_item:nth-child(5n) {
        margin-right: 0px;
    }

    .grid_item2, .grid_item3 {
        width: 120px !important;
        height: 95px;
        margin: 0px 5px 5px 0px;
        float: left;
        padding: 10px;
        font-size: 13px;
        cursor: pointer;
        word-break: normal;
        border-radius: 5px;
        
    }


    .grid_item_off {
        border: 2px solid #ffffff;
        color: #414040;
        background: #f8f8f8;
        background: -moz-linear-gradient(top, #f8f8f8 0%, #eeeff1 100%);
        background: -webkit-linear-gradient(top, #f8f8f8 0%, #eeeff1 100%);
        background: linear-gradient(to bottom, #f8f8f8 0%, #eeeff1 100%);
        filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#f8f8f8', endColorstr='#eeeff1',GradientType=0 );
        border-radius: 5px;    
    }

    .grid_item_on {
        border: 2px solid #d6e7f7;
        border-radius: 5px;
        background: #f2f8fd;
        background: -moz-linear-gradient(top, #f2f8fd 0%, #e3f0f9 100%);
        background: -webkit-linear-gradient(top, #f2f8fd 0%, #e3f0f9 100%);
        background: linear-gradient(to bottom, #f2f8fd 0%, #e3f0f9 100%);
        filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#f2f8fd', endColorstr='#e3f0f9',GradientType=0 );
        color:#414040;
    }

    .grid_item_on span,
    .grid_item_on p {
        color: #006bc9 !important;
    }
    
    /* 상품 그룹 - 수정완료 */
        .grid_group_title {
            height:15px;
            display:block;
            margin-bottom: 4px;
            font-size: 12px;
            color: #999999;
        }
/* 상품명 */
    .grid_title {
        overflow: hidden;
        text-overflow: ellipsis;
        -webkit-line-clamp: 2;
        display: -webkit-box;
        -webkit-box-orient: vertical;
        font-weight: 600;
        height: 26px;
        margin-bottom: 15px;
        color: #393c3f;
    }

    .grid_title:hover {
        color: #006BC9;
        word-break: normal;
        overflow:visible;
        display:block;
    }

    
/* 상품 가격 */
    .grid_price_title {
        display: block;
        white-space: pre;
        text-align: right;
        font-size: 13px;
        color: #393c3f;
    }

    .self_label {
        width: 40px !important;
        float:left;
        text-align: right;
        margin-right: 5px;
        color: var(--color-tp-text-gray);
    }
/* 결제수단선택 */
    .self_pay, .card_pay, .cash_pay {
        font-weight: 500;
        cursor: pointer;
    }
    .self_pay_on, .card_pay_on, .cash_pay_on {
        color: var(--color-tp-main-blue) !important;
        border-bottom: 1px solid var(--color-tp-main-blue) !important;
    }

    .self_pay:hover, .card_pay:hover {
        color: rgb(0, 107, 201) !important;
    }
    .cash_pay:hover {
        color: rgb(0, 107, 201) !important;
    }
    input.cash_pay:focus {
        font-weight: bolder !important;
    }
    .card_label {
        width: 60px !important;
        font-size: 12px;
        font-weight: bold;
        margin-right:10px;
        /* margin-bottom:10px; */
    }

    .card_input {
        background: white;
        font-size: 12px;
        width: 38px !important;
        height: 25px;
        border: 1px solid #eeeeee !important;
        border-radius: 3px;
        color: #393c3f !important;
        font-weight: normal !important;
        margin-right:3px;
    }

    .card_select {
        width: 60px !important;
        font-size: 12px;
        border: 1px solid #eeeeee !important;
        color: #393c3f !important;
        margin-right:5px;
    }
    .cash_receipt {
        display:none;
    }
    .info_label {
        display: inline-block;
        padding: 5px;
        font-weight: 400;
    }
    .info_input {
        padding: 5px; 
        height: 28px;
        text-align: left;
        font-size: 13px;
        border-radius: 5px; 
        border:1px solid #eeeeee !important;
        color: #393C3F;
        font-weight: normal !important;
    }

    .OrderPopup {        
        border-radius: 5px;
        /* border: 1px solid #F4F4F4; */
    }
    .order_payment {
        border: 1px solid rgb(238, 238, 238); 
        padding:8px; 
        border-top: none; 
        color: #393C3F;  
        height:40px; 
        border-radius: 0px 0px 5px 5px; 
        margin-bottom:10px; 
        background: rgba(0, 107, 201, 0.04);
    }
    .order_payment_text {
        margin-left: 10px; 
        font-weight: bold; 
        font-size: 20px; 
        line-height: 22px;
        letter-spacing: -0.01em;
    }
    .order_payment_price {
        display:flex; 
        float:right; 
        margin-right:10px;
    }
    .price {
        margin-right: 10px; 
        font-size: 20px; 
        font-weight: bold;
    }
/* 오른쪽 하단 결제옵션 버튼 */
    .bot_btn_on {
        background-color: #006bc9;
        color: #ffffff;
        border: none;
        height: 45px;
        border-radius: 5px;
    }

    .bot_btn_off {
        /* background: #E8E9EA; */
        background: #eff0f2;
    border: 1px solid rgba(57, 60, 63, 0.15);
    color: #393C3F;
    opacity: 0.6;
    }

    .ag-theme-alpine .ag-header-cell-label {  
        font-size: 11px !important;
    }
    .gray_bold {
        font-weight: 300;
    }

    .add_opacity {
        opacity: 0.6;
        pointer-events: none;
    }
    .before_opacity {
        border-radius: 20px;
        width: 70px;
        height: 30px;
        left: 20px;
        top: 4px;
        color: #393C3F !important;
        opacity: 0.6 !important;
        font-size: 13px;
        line-height: 19px;
        letter-spacing: 0.01em;
        text-align: center;
        background: #eff0f2 !important;
        border: 1px solid rgba(57, 60, 63, 0.15) !important;
        
    }

    .btn_search {
        border-radius: 20px;
        width: 70px;
        height: 30px;
        background: #6B7379;
        letter-spacing: 1.5px;
        font-size: 13px;
        color: #FFFFFF;
        font-weight: lighter;       
        line-height: 1px;
        /* padding: 4px 20px 5px 20px; */
        


    }
    .btn_blue {
        margin-left: 10px;
    }
    /* .btn_blue, .db_new {
        color: black;
        border-radius: 5px;
        background: white;
        filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#7cafe4', endColorstr='#608fbe', GradientType=0);
    } */

/* 보기 btn */
    .btn_see {
        color: #006BC9;
        border: 1px solid rgba(0, 107, 201, 0.1);
        background: #ffffff;
        height: 40px; 
        width:50%; 
        margin-right: 10px;
        border-radius:5px;
    /* filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#ffffff', endColorstr='#f3f3f3', GradientType=0); */
    }

    /* .btn_see:hover {
        background: rgba(0, 107, 201, 0.04); 
        background: -moz-linear-gradient(
        top,
        #f2f4f4 0%,
        #eeeded 50%,
        #e9eaea 50%,
        #e3e4e5 52%,
        #cecfcf 100%
    );
    background: -webkit-linear-gradient(
        top,
        #f2f4f4 0%,
        #eeeded 50%,
        #e9eaea 50%,
        #e3e4e5 52%,
        #cecfcf 100%
    );
    background: linear-gradient(
        to bottom,
        #f2f4f4 0%,
        #eeeded 50%,
        #e9eaea 50%,
        #e3e4e5 52%,
        #cecfcf 100%
    );
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#f2f4f4', endColorstr='#cecfcf', GradientType=0);

    } */

    .btn_send {
        color: rgb(0, 107, 201, 0.6);
        border: 1px solid rgba(0, 107, 201, 0.1);
        background: rgba(0, 107, 201, 0.05);
        height: 40px; 
        border-radius:5px;

        width:50%; 
    /* filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#f9f7f7', endColorstr='#cccbcb', GradientType=0); */
    }

    /* .btn_send:hover {
        background: rgba(0, 107, 201, 0.04); 
        background: -moz-linear-gradient(
        top,
        #cbcaca 0%,
        #c3c1c2 49%,
        #b3b2b3 51%,
        #9e9d9e 100%
    );
    background: -webkit-linear-gradient(
        top,
        #cbcaca 0%,
        #c3c1c2 49%,
        #b3b2b3 51%,
        #9e9d9e 100%
    );
    background: linear-gradient(
        to bottom,
        #cbcaca 0%,
        #c3c1c2 49%,
        #b3b2b3 51%,
        #9e9d9e 100%
    );
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#cbcaca', endColorstr='#9e9d9e', GradientType=0);
    } */

    .btn_resend {
        border-radius: 5px;
        font-size: 12px;
        width: 120px; 
        align-items: center; 
        font-weight: lighter;
        height: 25px;
        line-height:0px;
        margin-left:5px;
        /* filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#799cd4', endColorstr='#5880c1', GradientType=0); */
        background: #eff0f2;
        border: 1px solid rgba(57, 60, 63, 0.15);
        color: #393C3F;
}
/* 고객상세내역 */
    .cust_label {
        display: inline-block;
        padding-left: 20px;
        width: 110px;
        padding-top: 3px;
        font-weight: bold;
        
    }

    .cust_input {
        width: 170px;
        border: 1px solid #E1E1E1;
        color: #575757;
        height: 22px;
    }

    .price_label {
        width: 130px;
        padding: 0px 10px;
        font-weight: normal;
        color: rgba(57, 60, 63, 0.8);
    }

    .btn_select {
        font-size: 11px;
        margin-top: 3px;
        font-weight: normal;
        float:right;

    }

    .price_content2 {
        /* width: 170px; */
        height: 24px;
        font-size: 14px;
    }

    .price_content {
        width: 145px;
        height: 24px;
        font-size: 13px;
        font-weight: lighter !important;
        border: none !important;
    }

    .add_taxfree {
        background-color: #fafafa !important;
    }

    .set_bold {
        font-weight: bold !important;
    }

    .grid_margin3 {
        margin-right: 5px;
    }

    .grid_margin3:nth-child(3n) {
        margin-right: 0px !important;
    }
    .order_Quantity {
        
        border-radius: 5px;
        width: 32px;
        font-size: 20px;
        border: 1px solid rgba(0, 107, 201, 0.1);
        background: rgba(0, 107, 201, 0.04);
        height: 30px;
        color: rgb(0, 107, 201, 0.6);


        /* font-weight: lighter; */
        line-height: 1px;
    }
    .db_new {
        border-radius: 20px;
        width: 70px;
        height: 30px;
        font-size: 13px;
        line-height: 19px;
        letter-spacing: 0.01em;
        text-align: center;
        background-color: #006bc9;
        color: #ffffff;
        border: none;
        
        /* background: #807f7f;
        background: -moz-linear-gradient(top,  #807f7f 0%, #636262 100%);
        background: -webkit-linear-gradient(top,  #807f7f 0%,#636262 100%);
        background: linear-gradient(to bottom,  #807f7f 0%,#636262 100%);
        filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#807f7f', endColorstr='#636262',GradientType=0 ); */
    }
    .new_btn {
        height: 20px; 
        width:85px; 
        color: #006BC9; 
        font-weight:normal; 
        border:1px solid #006BC9; 
        background: #ffffff;
        border-radius:3px;
        line-height: 5px;
    }
     .box {   
        display: flex;
        flex-direction: row;
        align-items: center;    
        height: 10px;
        background: #FFFFFF;
        box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
        border-radius: 5px;
        padding: 0.8rem;
    }
    .box-label {
        font-family: 'Inter';
        font-style: normal;
        font-weight: 800;
        font-size: 12px;
        line-height: 1px;
        letter-spacing: -0.01em;
        color: #222222;
    }
    #titleimg {
        height: 24px;
        /* width: 211px; */
    }

</style>