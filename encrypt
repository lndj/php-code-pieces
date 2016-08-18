<?php
/**
 * @desc   字符串加密解密
 * @date   2016－08-17
 * @param string $str 要加密或解密的字符串
 * @param string $key 秘钥
 * @param string $operation 操作(D解密E加密)
 * @return string
 */
function encrypt($str, $key = '', $operation = 'E')
{
    $key = md5($key);
    //截取11位秘钥和字符串
    $limit_strkey = 11;
    //密匙簿的长度限制
    $limit_key = 256;
    $key_length = strlen($key);
    $str = $operation == 'D' ? base64_decode($str) : substr(md5($str . $key), 0, $limit_strkey) . $str;
    $string_length = strlen($str);
    $rndkey = $box = array();
    $result = '';
    //产生密匙列表
    for ($i = 0; $i < $limit_key; $i++) {
        $rndkey[$i] = ord($key[$i % $key_length]);
        $box[$i] = $i;
    }
    //打乱密匙列表
    for ($j = $i = 0; $i < $limit_key; $i++) {
        $j = ($j + $box[$i] + $rndkey[$i]) % $limit_key;
        $tmp = $box[$i];
        $box[$i] = $box[$j];
        $box[$j] = $tmp;
    }
    //对字符串加密
    for ($a = $j = $i = 0; $i < $string_length; $i++) {
        $a = ($a + 1) % $limit_key;
        $j = ($j + $box[$a]) % $limit_key;
        $tmp = $box[$a];
        $box[$a] = $box[$j];
        $box[$j] = $tmp;
        //从密匙列表得出密匙进行异或，再转成字符串
        $result .= chr(ord($str[$i]) ^ ($box[($box[$a] + $box[$j]) % $limit_key]));
    }
    if ($operation == 'D') {
        if (substr($result, 0, $limit_strkey) == substr(md5(substr($result, $limit_strkey) . $key), 0, $limit_strkey)) {
            return substr($result, $limit_strkey);
        } else {
            return '';
        }
    } else {
        //防止特殊字符丢失,base64编码,去掉base64补齐的=
        return str_replace('=', '', base64_encode($result));
    }
}