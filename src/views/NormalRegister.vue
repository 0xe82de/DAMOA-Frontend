<template>
  <!-- 요구사항 01. 회원가입 폼 구현 -->
  <form action="" method="post">
    <div>
      <label for="id">아이디</label>
      <input type="email" name="id" id="id" v-model="id" v-on:keyup="checkId" placeholder="이메일을 입력하세요.">
      <span>{{ checkMsgId }}</span>
    </div>
    <div>
      <label for="pw">비밀번호</label>
      <input type="password" name="password" id="pw" v-model="pw" v-on:keyup="checkPw" placeholder="비밀번호를 입력하세요.">
      <span>{{ checkMsgPw }}</span>
    </div>
    <div>
      <label for="samePw">비밀번호 확인</label>
      <input type="password" id="samePw" v-model="samePw" v-on:keyup="checkSamePw" placeholder="비밀번호를 입력하세요.">
      <span>{{ checkMsgSamePw }}</span>
    </div>
    <div>
      <label for="name">이름</label>
      <input type="text" name="name" id="name" v-model="name" v-on:keyup="checkName" placeholder="이름을 입력하세요." >
      <span>{{ checkMsgName }}</span>
    </div>
    <div>
      <label for="nickname">별명</label>
      <input type="text" name="nickname" id="nickname" v-model="nickname" v-on:keyup="checkNickname" placeholder="별명을 입력하세요." >
      <span>{{ checkMsgNickname }}</span>
    </div>
    <div>
      <label for="gender">성별</label>
      <input type="radio" name="gender" id="gender">남자
      <input type="radio" name="gender" id="gender">여자
    </div>
    <div>
      <label for="birth">생년월일</label>
      <input type="date" name="birth" id="birth">
    </div>
    <div>
      <label for="job">직업</label>
      <select name="job" id="job">
        <option v-for="(job, index) in jobs" :key="index">{{ job }}</option>
      </select>
    </div>
    <div>
      <label for="terms">약관 전체 동의</label>
      <input type="checkbox" v-model="isCheckAll" v-on:click="checkAll">
      <div v-for="(term, index) in terms" :key="index">
        <input type="checkbox" name="terms" v-bind:id="`term${index}`">
        <label v-bind:for="`term${index}`">{{ term.title }}</label>
        <textarea readonly v-bind:value="term.content" cols="30" rows="10"></textarea>
      </div>
    </div>
    <input type="submit" value="회원가입" v-on:click="register">
  </form>

  <hr>

  <!-- 구버전 -->
  <!--
  <section id="register">
      <label for="useremail">이메일</label>
      <input type="email" name="" id="useremail" placeholder="이메일을 입력하세요." @blur="emailDuplCheck">
      <span class="badge badge-danger mt-1" v-if="isEmailEmpty">{{alert_empty}}</span>
      <span class="badge badge-danger mt-1" v-else-if="!isEmailOk">이메일 형식이 다릅니다.</span>
      <span class="badge badge-danger mt-1" v-else-if="isEmailDupl">이미 사용중인 이메일입니다.</span>
      <span class="badge badge-danger mt-1" v-else>사용할 수 있는 이메일입니다.</span>
      <br>
      <label for="userpw">비밀번호</label>
      <input type="password" name="" id="userpw" placeholder="숫자 , 알파벳(소문자, 대문자), 특수문자 중 두 가지 이상을 조합하여 8자리 이상 작성하세요." @blur="pwCheck">
      <span class="badge badge-danger mt-1" v-if="isPasswordEmpty">{{alert_empty}}</span>
      <span class="badge badge-danger mt-1" v-if="passPassword">올바른 비밀번호입니다.</span>
      <span class="badge badge-danger mt-1" v-if="!availablePassword">비밀번호 규칙에 맞게 입력해주세요.</span>
      <br>
      <label for="userpwchk">비밀번호 확인</label>
      <input type="password" name="email" id="userpwchk" placeholder="위와 같은 비밀번호를 입력해주세요." @blur="pwDuplCheck">
      <span class="badge badge-danger mt-1" v-if="isPasswordCheckEmpty">{{alert_empty}}</span>
      <span class="badge badge-danger mt-1" v-if="duplPassword">비밀번호가 일치합니다.</span>
      <span class="badge badge-danger mt-1" v-if="notDuplPassword">비밀번호가 일치하지 않습니다.</span>
      <br>
      <label for="usernickname">닉네임</label>
      <input type="text" name="email" id="usernickname" placeholder="닉네임을 입력하세요." @blur="nicknameDuplCheck">
      <span class="badge badge-danger mt-1" v-if="isNickEmpty">{{alert_empty}}</span>
      <span class="badge badge-danger mt-1" v-else-if="isNickDupl">이미 사용중인 닉네임입니다.</span>
      <span class="badge badge-danger mt-1" v-else-if="isNickDupl===false">사용할 수 있는 닉네임입니다.</span>
      <br>
      <div style="height: 50px"></div>
      <input type="checkbox" v-model="selectAll" @click="select()" /> 모두 동의합니다.
      <br>
      <div v-for='option in options' class="checkbox" :key="option">
        <label>
          <input type="checkbox" :value="option.index" v-model="selected" @change='updateCheckall()'>{{ option.title }}
        </label>
        <textarea readonly :value="option.content" cols="30" rows="10"></textarea>
      </div>
      <div style="height: 50px"></div>
      <button @click="totalCheck">회원가입</button>
  </section>
  -->
</template>
<script>
export default {
  data() {
    return {
      checkMsgId: '', // 아이디 검사 결과 메시지
      checkMsgPw: '', // 비밀번호 검사 결과 메시지
      checkMsgSamePw: '', // 동일 비밀번호 검사 결과 메시지
      checkMsgName: '', // 이름 검사 결과 메시지
      jobs: ["직장인", "대학생", "고등학생", "취준생", "자영업자"],
      terms: [
        {
          title: 'DAMOA 서비스 이용약관 동의 (필수)',
          content: 'DAMOA 서비스 약관',
          code: 'TERM01'
        },
        {
          title: '개인정보 수집 및 이용 동의 (필수)',
          content: '개인정보 수집 및 이용 약관',
          code: 'TERM02'
        },
        {
          title: '위치정보 수집 및 이용 동의 (선택)',
          content: '위치정보 수집 및 이용 약관',
          code: 'TERM03'
        },
        {
          title: '프로모션 정보 수신 동의 (선택)',
          content: '프로모션 정보 수신 약관',
          code: 'TERM04'
        }
      ],
      id: '',
      pw: '',
      samePw: '',
      name: '',
      nickname: '',

      // 아이디, 비밀번호 정규표현식
      regExpId: /[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*.[a-zA-Z]$/i,

      URL_ID_DUPL_CHECK: 'https://c4463872-d5df-4f02-ad5c-cf7d329eeac2.mock.pstmn.io/checkIdDupl',

      // 구버전
      // 이용약관 동의 내용
      // options: [
      //   {title: "DAMOA 이용약관 동의(필수)", content: "약관 내용 1", index: "CK01"},
      //   {title: "개인정보 수집 및 이용 동의(필수)", content: "약관 내용 2", index: "CK02"},
      //   {title: "위치정보 이용약관 동의(선택)", content: "약관 내용 3", index: "CK03"},
      //   {title: "프로모션 정보 수신 동의(선택)", content: "약관 내용 4", index: "CK04"}
      // ],
      // selected: [],
      // selectAll: false,

      // alert_empty: '',

      // // availableEmail: true,
      // isEmailOk: true,
      // isEmailEmpty: true,
      // isEmailDupl: true,

      // // passEmail: false,
      // isPasswordEmpty: true,
      // isPasswordCheckEmpty: true,
      // availablePassword: true,
      // passPassword: false,
      // duplPassword: false,
      // notDuplPassword: false,

      // // nickname 변수
      // isNickEmpty: true,
      // isNickDupl: null,

      // urlEmailDuplCheck: 'https://c4463872-d5df-4f02-ad5c-cf7d329eeac2.mock.pstmn.io/emailDuplCheck',
      // urlNickDuplCheck: 'https://c4463872-d5df-4f02-ad5c-cf7d329eeac2.mock.pstmn.io/nickDuplCheck'
    };
  },
  methods: {
    // 요구사항 02, 03
    checkId() {
      // 요구사항 02. 아이디 중복 검사
      if (this.regExpId.test(this.id)) {
        // 요구사항 03. 아이디 중복 검사
        const requestOptions = {
            method: 'GET',
            headers: {'Content-Type': 'application/json'},
        };
        fetch(this.URL_ID_DUPL_CHECK + '?id=' + this.id, requestOptions)
            .then(async response => {
              const data = await response.json();
              if (data.isIdDupl) {
                this.checkMsgId = '이미 사용중인 아이디입니다.';
              }
              else {
                this.checkMsgId = '사용하실 수 있는 아이디입니다.';
              }

              // check for error response
              if (!response.ok) {
                // get error message from body or default to response status
                const error = (data && data.message) || response.status;
                return Promise.reject(error);
              }
            })
            .catch(error => {
              this.errorMessage = error;
              console.error('There was an error!', error);
            });
      }
      else {
        this.checkMsgId = '이메일 형식에 맞게 입력해주세요.'
      }
    },
    // 요구사항 05. 비밀번호 유효성 검사
    checkPw() {
      // 비밀번호 길이 검사
      this.samePw = '';
      this.checkSamePw = '';
      if (this.pw.length < 8) {
        this.checkMsgPw = '비밀번호는 8자 이상 입력해주세요.'
      }
      else {
        /**
         * 비밀번호 패턴 검사
         * 영대문자, 영소문자, 숫자, 특수문자 중 3가지 이상 포함 필요
         */
        
        // 비밀번호 가져오기
        const tmpPw = this.pw;

        // 영대문자, 영소문자, 숫자, 특수문자 체크
        const cEngCheck = tmpPw.search(/[A-Z]/g)
        const sEngCheck = tmpPw.search(/[a-z]/g);
        const numCheck = tmpPw.search(/[0-9]/g);
        const speCheck = tmpPw.search(/[`~!@@#$%^&*|₩₩₩'₩";:₩/?]/gi);

        let patternCnt = 0;
        if (numCheck >= 0) ++patternCnt;
        if (sEngCheck >= 0) ++patternCnt;
        if (cEngCheck >= 0) ++patternCnt;
        if (speCheck >= 0) ++patternCnt;

        if (patternCnt >= 3) this.checkMsgPw = '사용하실 수 있는 비밀번호입니다.'
        else this.checkMsgPw = '비밀번호는 영대문자, 영소문자, 숫자, 특수문자 중 세 가지 이상 포함하여 입력해주세요.'
      }
    },
    // 요구사항 06. 동일 비밀번호 확인
    checkSamePw() {
      if (this.samePw.length >= 8) {
        if (this.pw === this.samePw) {
          this.checkMsgSamePw = '비밀번호가 일치합니다.';
        }
        else {
          this.checkMsgSamePw = '동일한 비밀번호를 입력해주세요.';
        }
      }
    },
    // 요구사항 07. 이름 유효성 검사
    checkName() {
      const tmpName = this.name;
      const cEngCheck = tmpName.search(/[A-Z]/g)
      const sEngCheck = tmpName.search(/[a-z]/g);
      const numCheck = tmpName.search(/[0-9]/g);
      const speCheck = tmpName.search(/[`~!@@#$%^&*|₩₩₩'₩";:₩/?]/gi);
      if (cEngCheck + sEngCheck + numCheck + speCheck > -4) {
        this.checkMsgName = '이름은 한글만 입력해주세요.';
      }
      else {
        this.checkMsgName = '';
      }
    },
    // 요구사항 08, 09
    checkNickname() {
      // 요구사항 08. 별명 유효성 검사
      const tmpNickname = this.nickname;
      const speCheck = tmpNickname.search(/[`~!@@#$%^&*|₩₩₩'₩";:₩/?]/gi);
      if (speCheck > -1) {
        this.checkMsgNickname = '별명에 특수문자를 사용할 수 없습니다.';
      }
      else {
        // 요구사항 09. 별명 중복 검사
        /*
        서버 통신 필요
        */
        this.checkMsgNickname = '사용하실 수 있는 별명입니다.';
      }
    },

    select() {
      this.selected = [];
      if (!this.selectAll) {
        for (let i in this.options) {
          this.selected.push(this.options[i].index);
        }
      }
    },
    updateCheckall: function () {
      if (this.options.length == this.selected.length) {
        this.selectAll = true;
      } else {
        this.selectAll = false;
      }
    },
    async emailDuplCheck() {
      // 이메일 검증 변수 초기화

      // 이메일 가져오기
      const userEmail = document.getElementById("useremail").value;

      this.isEmailEmpty = true;
      this.isEmailOk = true;
      if (userEmail.length > 0) {
        this.isEmailEmpty = false;

        // 정규식
        const reg_email = /[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_.]?[0-9a-zA-Z])*.[a-zA-Z]$/i;

        // 이메일 형식이 맞으면
        if (reg_email.test(userEmail)) {
          const requestOptions = {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: JSON.stringify({email: userEmail})
          };
          fetch(this.urlEmailDuplCheck, requestOptions)
              .then(async response => {
                const data = await response.json();
                this.isEmailDupl = data.isDupl;

                // check for error response
                if (!response.ok) {
                  // get error message from body or default to response status
                  const error = (data && data.message) || response.status;
                  return Promise.reject(error);
                }
              })
              .catch(error => {
                this.errorMessage = error;
                console.error('There was an error!', error);
              });
        }
        // 이메일 형식이 안맞으면
        else {
          this.isEmailOk = false;
          this.isEmailDupl = true;
        }
      } else {
        this.isEmailEmpty = true;
        this.isEmailDupl = true;
      }
    },
    pwCheck() {
      const userPassword = document.getElementById("userpw").value;
      this.passPassword = false;
      this.availablePassword = true;
      if (userPassword.length > 0) {
        this.isPasswordEmpty = false;
        this.availablePassword = true;
        // this.passEmail = false;
        const numIdx = userPassword.search(/[0-9]/g);
        const sEngIdx = userPassword.search(/[a-z]/g);
        const cEngIdx = userPassword.search(/[A-Z]/g)
        const speIdx = userPassword.search(/[`~!@@#$%^&*|₩₩₩'₩";:₩/?]/gi);

        let count = 0;
        if (numIdx < 0) --count;
        if (sEngIdx < 0) --count;
        if (cEngIdx < 0) --count;
        if (speIdx < 0) --count;

        if (count < -2 || userPassword.length < 8) this.availablePassword = false;
        else this.availablePassword = true;
        this.passPassword = this.availablePassword
      } else {
        this.isPasswordEmpty = true;
      }
    },
    pwDuplCheck() {
      const pw1 = document.getElementById("userpw").value;
      const pw2 = document.getElementById("userpwchk").value;

      if (pw2.length == 0) {
        this.isPasswordCheckEmpty = true;
        this.duplPassword = false;
        this.notDuplPassword = false;
      } else if (pw1 === pw2) {
        this.duplPassword = true;
        this.notDuplPassword = false;
        this.isPasswordCheckEmpty = false;
      } else {
        this.isPasswordCheckEmpty = false;
        this.duplPassword = false;
        this.notDuplPassword = true;
      }
    },
    async nicknameDuplCheck() {
      // 닉네임 가져오기
      const nickname = document.getElementById("usernickname").value;
      this.isNickDupl = null;
      this.isNickEmpty = true;
      if (nickname.length > 0) {
        this.isNickEmpty = false;

        // 서버에서 체크하고 값을 받아오는 코드
        // ...
        const requestOptions = {
          method: 'POST',
          headers: {'Content-Type': 'application/json'},
          body: JSON.stringify({nickname: nickname})
        };
        fetch(this.urlNickDuplCheck, requestOptions)
            .then(async response => {
              const data = await response.json();
              this.isNickDupl = data.isDupl;

              // check for error response
              if (!response.ok) {
                // get error message from body or default to response status
                const error = (data && data.message) || response.status;
                return Promise.reject(error);
              }
            })
            .catch(error => {
              this.errorMessage = error;
              console.error('There was an error!', error);
            });
        // ...
      } else {
        this.isNickEmpty = true;
        this.isNickDupl = null;
      }
    },
    totalCheck() {
      if (
          !this.isEmailDupl && this.duplPassword &&
          !this.isNickDupl &&
          this.selected.includes("CK01") && this.selected.includes("CK02")
      ) {
        // 이메일, 비밀번호,  닉네임, 약관동의
        const data = {
          email: document.getElementById("useremail").value,
          password: document.getElementById("userpw").value,
          nickname: document.getElementById("usernickname").value,
          agree: this.selected
        };
        // data.email = document.getElementById("useremail").value;
        // data.password = document.getElementById("userpw").value;
        // data.nickname = document.getElementById("usernickname").value;
        // data.agree = this.selected;
        console.log(data);
        // postman 테스트 해서
        // const response = await axios.post("url", data);

        // 성공

        // 실패
        alert('성공');
      } else {
        this.alert_empty = '필수 정보입니다.'
      }
    }
  }
}

</script>