<!doctype html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>jQuery密码强度验证</title>


<link href='css/style.css' rel='stylesheet' type='text/css'>

</head>

<body>


<form id="myform">
	<p>
		<label>输入一个测试密码</label>
		<input type="password" id="myPassword" name="" value="">
	</p>
</form>

<script type="text/javascript" src="js/jquery.min.js"></script>
<script type="text/javascript" src="js/strength.js"></script>
<script type="text/javascript">
$(document).ready(function($) {
		
	$('#myPassword').strength({
		strengthClass: 'strength',
		strengthMeterClass: 'strength_meter',
		strengthButtonClass: 'button_strength',
		strengthButtonText: '显示密码',
		strengthButtonTextToggle: '隐藏密码'
	});
	
});
</script>

</body>
</html>

<style>
  @charset "utf-8";
/* CSS Document */
html{color:#000;background:#FFF;}body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,form,fieldset,legend,input,textarea,p,blockquote,th,td{margin:0;padding:0;}table{border-collapse:collapse;border-spacing:0;}fieldset,img{border:0;}address,caption,cite,code,dfn,em,strong,th,var{font-style:normal;font-weight:normal;}li{list-style:none;}caption,th{text-align:left;}h1,h2,h3,h4,h5,h6{font-size:100%;font-weight:normal;}q:before,q:after{content:'';}abbr,acronym{border:0;font-variant:normal;}sup{vertical-align:text-top;}sub{vertical-align:text-bottom;}input,textarea,select{font-family:inherit;font-size:inherit;font-weight:inherit;}input,textarea,select{*font-size:100%;}legend{color:#000;}
/*SITE STYLING*/
html{ background:#39C;font-family:'Lato', sans-serif;color:#000;}
.clear{clear:both;display:block;}

p{color:#777;line-height:50px;}

/* myform */
#myform{width:400px;margin:50px auto 0 auto;position:relative; color:#000;}
#myform label{margin-bottom:5px;display:block;text-transform:uppercase;font-size:14px;color:#000;font-weight:bold;}
#myform input[type="password"],#myform input[type="text"]{
	background:transparent;border:2px solid #46AC84;
	color:#777;
	font-family:"Lato", sans-serif;
	font-size:14px;
	padding:9px 5px;
	height:21px;表单
	text-indent:6px;
	-webkit-appearance:none;
	-webkit-border-radius:6px;
	-moz-border-radius:6px;
	border-radius:6px;
	-webkit-box-shadow:none;
	-moz-box-shadow:none;
	box-shadow:none;
	-webkit-transition:border .25s linear, color .25s linear;
	-moz-transition:border .25s linear, color .25s linear;
	-o-transition:border .25s linear, color .25s linear;
	transition:border .25s linear, color .25s linear;
	-webkit-backface-visibility:hidden;
	width:100%;
}
#myform input[type="password"]:focus,#myform input[type="text"]:focus{outline:0;border:2px solid white;}

.strength_meter{
	position:absolute;
	left:0px;
	top:60px;
	width:100%;
	height:43px;
	z-index:-1;
	border-radius:5px;
}
.button_strength {
	text-decoration:none;
	color:#000;
	font-size:13px;
}
.strength_meter div{width:0%;
	height:43px;
	text-align:right;
	color:#000;
	line-height:43px;
	-webkit-transition:all .3s ease-in-out;
	padding-right:12px;
	border-radius:5px;
}
.strength_meter div p{
	position:absolute;
	top:48px;
	right:-11px;
	color:#000;
	font-size:13px;
}

.veryweak{
	background-color:#FFA0A0;
	border-color:#F04040!important;
	width:25%!important;
}
.weak{
	background-color:#FFB78C;
	border-color:#FF853C!important;
	width:50%!important;
}
.medium{
	background-color:#FFEC8B;
	border-color:#FC0!important;
	width:75%!important;
}
.strong{
	background-color:#C3FF88;
	border-color:#8DFF1C!important;
	width:100%!important;
}
</style>
<script src="http://www.jq22.com/jquery/jquery-1.10.2.js"></script>
<script>
  /*!
 * strength.js
 * Original author: @aaronlumsden
 * Further changes, comments: @aaronlumsden
 * Licensed under the MIT license
 */
;(function ( $, window, document, undefined ) {

    var pluginName = "strength",
        defaults = {
            strengthClass: 'strength',
            strengthMeterClass: 'strength_meter',
            strengthButtonClass: 'button_strength',
            strengthButtonText: 'Show Password',
            strengthButtonTextToggle: 'Hide Password'
        };

       // $('<style>body { background-color: red; color: white; }</style>').appendTo('head');

    function Plugin( element, options ) {
        this.element = element;
        this.$elem = $(this.element);
        this.options = $.extend( {}, defaults, options );
        this._defaults = defaults;
        this._name = pluginName;
        this.init();
    }

    Plugin.prototype = {

        init: function() {


            var characters = 0;
            var capitalletters = 0;
            var loweletters = 0;
            var number = 0;
            var special = 0;

            var upperCase= new RegExp('[A-Z]');
            var lowerCase= new RegExp('[a-z]');
            var numbers = new RegExp('[0-9]');
            var specialchars = new RegExp('([!,%,&,@,#,$,^,*,?,_,~])');

            function GetPercentage(a, b) {
                    return ((b / a) * 100);
                }

                function check_strength(thisval,thisid){
                     if (thisval.length > 8) { characters = 1; } else { characters = 0; };
                    if (thisval.match(upperCase)) { capitalletters = 1} else { capitalletters = 0; };
                    if (thisval.match(lowerCase)) { loweletters = 1}  else { loweletters = 0; };
                    if (thisval.match(numbers)) { number = 1}  else { number = 0; };

                    var total = characters + capitalletters + loweletters + number + special;
                    var totalpercent = GetPercentage(7, total).toFixed(0);

                  

                    get_total(total,thisid);
                }

            function get_total(total,thisid){

                  var thismeter = $('div[data-meter="'+thisid+'"]');
                if(total == 0){
                      thismeter.removeClass().html('');
                }else if (total <= 1) {
                   thismeter.removeClass();
                   thismeter.addClass('veryweak').html('<p>强度: 很弱</p>');
                } else if (total == 2){
                    thismeter.removeClass();
                   thismeter.addClass('weak').html('<p>强度: 较弱</p>');
                } else if(total == 3){
                    thismeter.removeClass();
                   thismeter.addClass('medium').html('<p>强度: 一般</p>');

                } else {
                     thismeter.removeClass();
                   thismeter.addClass('strong').html('<p>强度: 强</p>');
                } 
                /*console.log(total);*/
            }





            var isShown = false;
            var strengthButtonText = this.options.strengthButtonText;
            var strengthButtonTextToggle = this.options.strengthButtonTextToggle;


            thisid = this.$elem.attr('id');

            this.$elem.addClass(this.options.strengthClass).attr('data-password',thisid).after('<input style="display:none" class="'+this.options.strengthClass+'" data-password="'+thisid+'" type="text" name="" value=""><a data-password-button="'+thisid+'" href="" class="'+this.options.strengthButtonClass+'">'+this.options.strengthButtonText+'</a><div class="'+this.options.strengthMeterClass+'"><div data-meter="'+thisid+'"><p></p></div></div>');
             
            this.$elem.bind('keyup keydown', function(event) {
                thisval = $('#'+thisid).val();
                $('input[type="text"][data-password="'+thisid+'"]').val(thisval);
                check_strength(thisval,thisid);
                
            });

             $('input[type="text"][data-password="'+thisid+'"]').bind('keyup keydown', function(event) {
                thisval = $('input[type="text"][data-password="'+thisid+'"]').val();
                /*console.log(thisval);*/
                $('input[type="password"][data-password="'+thisid+'"]').val(thisval);
                check_strength(thisval,thisid);
                
            });



            $(document.body).on('click', '.'+this.options.strengthButtonClass, function(e) {
                e.preventDefault();

               thisclass = 'hide_'+$(this).attr('class');




                if (isShown) {
                    $('input[type="text"][data-password="'+thisid+'"]').hide();
                    $('input[type="password"][data-password="'+thisid+'"]').show().focus();
                    $('a[data-password-button="'+thisid+'"]').removeClass(thisclass).html(strengthButtonText);
                    isShown = false;

                } else {
                    $('input[type="text"][data-password="'+thisid+'"]').show().focus();
                    $('input[type="password"][data-password="'+thisid+'"]').hide();
                    $('a[data-password-button="'+thisid+'"]').addClass(thisclass).html(strengthButtonTextToggle);
                    isShown = true;
   
                }


               
            });


         
            
        },

        yourOtherFunction: function(el, options) {
            // some logic
        }
    };

    // A really lightweight plugin wrapper around the constructor,
    // preventing against multiple instantiations
    $.fn[pluginName] = function ( options ) {
        return this.each(function () {
            if (!$.data(this, "plugin_" + pluginName)) {
                $.data(this, "plugin_" + pluginName, new Plugin( this, options ));
            }
        });
    };

})( jQuery, window, document );



</script>
