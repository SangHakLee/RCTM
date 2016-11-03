# How to get all data using ajax post in Codeigniter

- 다음과 같이 하면 form에서 name으로 넘어온 데이터를 array()로 받는다.
- post()의 2번째 인자에 true 넣어주면 xxs 스크립팅 공격 막는다.

```bash
$post_data = $this->input->post(null, true);
```
