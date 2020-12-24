<div class="cnblogs_Highlighter">
<pre class="brush:html;gutter:true;"> var formdata = {
            "type": 1,
            "category": 1,
            "name": "张强",
            "mobile": 13540444605,
            "province": 110000,
            "city": 110100,
            "area": 110101,
            "user_remark": "我还可以更详细的描述我手机的故障情况",
            "phone_id": 67,
            "color_id": 1,
            "areaText": "东城区",
            "provinceText": "北京市",
            "cityText": "北京市",
            "is_invoice": 0,
            "is_personal": 1,
            "detailed_address": "北京市清华大学",
            "phone_imei": "",
            "date": "",
            "malfunctions[]": 1057,
            "malfunctions[]": 1060,
            "malfunctions[]": 1204,
            "malfunctions[]": 1191,
            "mailAddress": "",
            "mailCiyt": "",
            "tommagic": "",
            "reference_price": 979,
            "landUrl": "http://www.weadoc.com/successTips.html",
            "referrer": "http: //www.weadoc.com/fixWay.html",
            "is_period": "",
            "brand_id": 1
        };

        function Test() {
            $.ajax({
                type: "post",
                url: "http://api.shanxiuxia.com/api/order/add",
                data: formdata,
                async: true,
                success: function(data) {
                    console.log(data);
                }
            });
        }
</pre>
</div>
<p>　　后期完善：</p>
<p>　　　　1.判断哪些参数是必须输入，简化参数；</p>
<p>　　　　2.使用代理IP,防止被IP被黑或者监视。</p>