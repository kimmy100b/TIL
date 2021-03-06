# vee-validation

스터디 커뮤니티 사이트의 회원가입 기능을 구현한 후 vee-validation을 이용하여 유효성 검사를 할 때 문제점

## email 형식인지 체크하기

```javascript
<validation-provider
  name="이메일"
  :rules="{ email: true }"
  v-slot="validationContext"
>
  <b-form-group>
    <label>이메일(선택)</label>
    <b-form-input
      name="userEmail"
      v-model="user.email"
      :state="getValidationState(validationContext)"
      size="lg"></b-form-input>
    <b-form-invalid-feedback>
      {{ validationContext.errors[0] }}
    </b-form-invalid-feedback>
  </b-form-group>
</validation-provider>
```

## 비밀번호 재확인

진짜 많이 삽질했던 부분(js로 내가 만들어줄까도 고민했다ㅋㅋㅋㅋ)<br>
가장 큰 문제는 vid였다.<br>
vid와 confirmed의 내용이 같아야한다.<br>
여기서 그냥 vid를 입력하면 아무리 내용을 같게 입력해줘도 일치하지 않는다는 것이 문제!<br>
`:vid`로 입력해서 동적으로 입력받게 해주었다.<br>
그리고 vid뒤에 내용이 내가 임의로 설정하지 말고 안에 들어가는 데이터를 담은 `user.pwd`로 입력해주어야한다.

```javascript
<validation-provider
  name="비밀번호"
  :rules="{
  required: true,
  min:8,
  max:16
  }"
  v-slot="validationContext"
  :vid="user.pwd"
>
  <b-form-group
    description="8~16자 영문 대 소문자, 숫자, 특수문자를 사용하세요."
  >
    <label>비밀번호</label>
    <b-form-input
      id="userPwd"
      type="password"
      v-model="user.pwd"
      :state="getValidationState(validationContext)"
      size="lg"
    ></b-form-input>
    <b-form-invalid-feedback>
      {{ validationContext.errors[0] }}
    </b-form-invalid-feedback>
  </b-form-group>
</validation-provider>

<validation-provider
  name="비밀번호확인"
  :rules="{ required: true, confirmed: user.pwd }"
  v-slot="validationContext"
>
  <b-form-group>
    <label>비밀번호 재확인</label>
    <b-form-input
      type="password"
      v-model="pwdConfirmation"
      :state="getValidationState(validationContext)"
      size="lg"></b-form-input>
    <b-form-invalid-feedback>
      {{ validationContext.errors[0] }}
    </b-form-invalid-feedback>
  </b-form-group>
</validation-provider>
```

참고

- [교차필드검증](https://vee-validate.logaretm.com/v2/guide/components/validation-provider.html#input-groups-checkbox-radio)
- [코드](https://codesandbox.io/s/yw8yrmn9y9?file=/src/components/Form.vue)
