# Strange witre spaces in textarea with PHP

- html 내용이 길어지면 textarea의 내용을 보기 좋게 하려고 다음과 같이 표현 할 때가 있다.

```html
    <table>
        <tbody>
            <tr>
                <textarea rows="8" cols="80">
                    <?php $something_foo_var_contents ?>
                </textarea>
            </tr>
        </tbody>
    </table>
```
> 사진
![bad_textarea](bad_textarea.png)

**textarea는 <> 과 </> 사이의 모든 내용을 표시하므로 줄바꿈을 하면 해당 내용도 추가된다.**

- 조금 길어져도 붙여서 사용해야 한다.
```html
    <textarea rows="8" cols="80"><?php $something_foo_var_contents ?></textarea>
```
