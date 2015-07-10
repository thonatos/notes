Android zip/apk 签名脚本

反编译、定制修改Android ROM或者程序的时候，需要用到签名工具，使用方法自行搜索。

下面是在linux上可以用得一个sh脚本。

#### Script

	#!/bin/sh

	# ====Sign Function 
	# **	input file with name like "file.zip/file.apk"
	# **	output file with name like "file_signde.zip/file_signed.apk" 
	Sign(){
		f_i=$1;
		f_o=$2;
		jar_dir=$3;
		java -jar $jar_dir/SignApk.jar $jar_dir/testkey.x509.pem $jar_dir/testkey.pk8 $f_i $f_o;
	}

	# ====Main Function
	# **	Parm : "file.zip/file.apk"
	# **	Useage: 	"Signtool file.zip / Signtool file.apk"
	echo "\n";
	echo "\t--------===== SignTool For Apk/Zip =====--------";
	echo "\t\t\t——http://www.thonatos.com";
	echo "\t\t\t——By.Thonatos.Yang.2014.07.19\n";

	# Define The var
	in=$1;
	out_name=${in%%.*} ;
	out_suffix=${in##*.}  ;
	out_file=${out_name}"_signed."${out_suffix};
	sign_jar=$(cd "$(dirname "$0")"; pwd);

	# Print The Sign Info 
	echo "==:Input	: $out_name";
	echo "==:Type		: $out_suffix";
	echo "==:Output	: $out_file";

	# Sign The File
	Sign $in $out_file $sign_jar;
	wait ;
	echo "==:Result	: Sign Successful !\n";
	exit;