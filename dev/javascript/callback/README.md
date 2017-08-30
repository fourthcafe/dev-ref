# javascript callback 함수에서 this 처리

```javascript
var jsObj = function() {
	this.callAjax = function() {
		$.ajax({
			type : 'post',
			dataType : 'json',
			url : url,
			data : params,
			context: this,		// 1. context로 this를 넘겨줌
			beforeSend : function(xhr){
				this.template.find('[data-function="cancelClass"]').attr('disabled', true);
				this.template.find('[data-function="cancelClass"]').text('처리중...');
			},
			success : function(returnData) {
				...
			},
			error : function(request, status, error) {
				...
			},
			complete : function(xhr, textStatus) {
				this.close(function(context) {		// 5. context 매개변수 사용
					context.template.find('[data-function="cancelClass"]').attr('disabled', false);
					context.template.find('[data-function="cancelClass"]').text('수업 취소');
					schedulePopup.resetReload();
				}, this);	// 2. close를 호출하며 두번째 인자로 this 넘겨줌
			}
		});
	},


	// 3. 넘겨받은 매개변수 중 this의 변수 이름을 context로 지정
	this.close = function(cbFunc, context) {
		this.template.fadeOut(300, function() {
			if (typeof cbFunc === 'function') {
				cbFunc(context);	// 4. callback함수에 context를 매개변수로 넘겨줌
			}
		});
	}
}
```
