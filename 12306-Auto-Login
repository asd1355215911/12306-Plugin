/*
		12306 Auto Login => A javascript snippet to help you auto login 12306.com.
		Copyright (C) 2011 Kevintop
		
		Includes jQuery
		Copyright 2011, John Resig
		Dual licensed under the MIT or GPL Version 2 licenses.
		http://jquery.org/license

		Includes 12306.user.js
		https://gist.github.com/1554666
		Copyright (C) 2011 Jingqin Lynn
		Released GNU Licenses.

		This program is free software: you can redistribute it and/or modify
		it under the terms of the GNU General Public License as published by
		the Free Software Foundation, either version 3 of the License, or
		(at your option) any later version.

		This program is distributed in the hope that it will be useful,
		but WITHOUT ANY WARRANTY; without even the implied warranty of
		MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
		GNU General Public License for more details.

		You should have received a copy of the GNU General Public License
		along with this program.  If not, see <http://www.gnu.org/licenses/>.

*/

// ==UserScript==  
// @name         12306 Auto Login  
// @author       kevintop@gmail.com  
// @namespace    https://plus.google.com/107416899831145722597  
// @description  A javascript snippet to help you auto login 12306.com
// @include      *://dynamic.12306.cn/otsweb/loginAction.do*
// @include      *://dynamic.12306.cn/otsweb/login.jsp*
// ==/UserScript== 
function withjQuery(callback, safe){
	if(typeof(jQuery) == "undefined") {
		var script = document.createElement("script");
		script.type = "text/javascript";
		script.src = "https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js";

		if(safe) {
			var cb = document.createElement("script");
			cb.type = "text/javascript";
			cb.textContent = "jQuery.noConflict();(" + callback.toString() + ")(jQuery);";
			script.addEventListener('load', function() {
				document.head.appendChild(cb);
			});
		}
		else {
			var dollar = undefined;
			if(typeof($) != "undefined") dollar = $;
			script.addEventListener('load', function() {
				jQuery.noConflict();
				$ = dollar;
				callback(jQuery);
			});
		}
		document.head.appendChild(script);
	} else {
		callback(jQuery);
	}
}
withjQuery(function($){
	var url = "https://dynamic.12306.cn/otsweb/loginAction.do?method=login";
	var queryurl = "https://dynamic.12306.cn/otsweb/order/querySingleAction.do?method=init";
	function submitForm(){
		var submitUrl = url + "&loginUser.user_name=" + $("#UserName").val() + "&user.password=" + $("#password").val() + "&randCode=" + $("#randCode").val();
		$.ajax({
			type: "POST",
			url: submitUrl,
			cache: false,
			timeout:15000,
			success: function(msg){
				if (msg.indexOf('请输入正确的验证码') > -1) {
					alert('请输入正确的验证码，并重新点自动登录！');
				}else if (msg.indexOf('当前访问用户过多') > -1) {
					reLogin();
				}else {
					$('#reloginNum').html('登录成功，请点击车票预订！');
					location.replace(queryurl);
				};
			},
			error: function(msg){
				reLogin();
			},
			beforeSend: function(XHR){
				//alert("Data Saved: " + XHR);
			}
		});
	}

	function reLogin(){
		var currNum = parseInt($('#reloginNum').html());
		if(isNaN(currNum)){
			currNum = 0;
		}
		$('#reloginNum').html(currNum + 1);
		submitForm();
	}
	//初始化
	if($("#autoSubmit").size()<1){
		$("body").prepend("<div style='white-space:nowrap;'>重试次数:<span id='reloginNum' >0</span></div>");
		$("#subLink").after("<a href='#' id='autoSubmit' class='button_a' ><span><ins>自动登录</ins> </span> </a>");
		$("#autoSubmit").live("click", function(){
			alert('开始尝试登录，点确定后，请耐心等待！');
			submitForm();
		});
		alert('如果使用自动登录功能，请输入用户名、密码及验证码后，点击自动登录，系统会尝试登录，直至成功！');
	}
}, true);
