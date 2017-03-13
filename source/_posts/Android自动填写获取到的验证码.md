---
title: Android自动填写获取到的验证码
date: 2017-03-13 22:26:40
categories: android
tags: [anroid,自动填写获取到的验证码]
---
## 添加权限
``` xml
<uses-permission android:name="android.permission.RECEIVE_SMS"></uses-permission>
<uses-permission android:name="android.permission.READ_SMS"></uses-permission>
```
<!-- more -->
## 示例代码
``` java
public class SMSReceiver extends BroadcastReceiver
{
    public interface ISMSListener {
        public void onSmsReceive(String verifyCode);
    }

    private static ISMSListener mSMSListener;

    public SMSReceiver(ISMSListener ismsListener) {
        mSMSListener = ismsListener;
    }

    public static final String TAG = "ImiChatSMSReceiver";

    //android.provider.Telephony.Sms.Intents

    public static final String SMS_RECEIVED_ACTION = "android.provider.Telephony.SMS_RECEIVED";


    @Override
    public void onReceive(Context context, Intent intent)
    {
        LogUtil.d(">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>");
        if (intent.getAction().equals(SMS_RECEIVED_ACTION))
        {
            SmsMessage[] messages = getMessagesFromIntent(intent);
            for (SmsMessage message : messages)
            {

//                LogUtil.d(message.getOriginatingAddress() + " : " +
//                        message.getDisplayOriginatingAddress() + " : " +
//                        message.getDisplayMessageBody() + " : " +
//                        message.getTimestampMillis());


                String msg = message.getDisplayMessageBody();
                LogUtil.d("MSG: " + msg);
                String verifyCode = null;
                Pattern p = Pattern.compile("\\d{4}");
                Matcher m = p.matcher(msg);
                while (m.find()) {
                    verifyCode = m.group();
                    break;
                }
                LogUtil.d("verifyCode " + verifyCode);
                if (mSMSListener != null) {
                    mSMSListener.onSmsReceive(verifyCode);
                }
            }

        }

    }


    public final SmsMessage[] getMessagesFromIntent(Intent intent)
    {
        Object[] messages = (Object[]) intent.getSerializableExtra("pdus");
        byte[][] pduObjs = new byte[messages.length][];
        for (int i = 0; i < messages.length; i++)
        {
            pduObjs[i] = (byte[]) messages[i];
        }
        byte[][] pdus = new byte[pduObjs.length][];
        int pduCount = pdus.length;
        SmsMessage[] msgs = new SmsMessage[pduCount];
        for (int i = 0; i < pduCount; i++)
        {
            pdus[i] = pduObjs[i];
            msgs[i] = SmsMessage.createFromPdu(pdus[i]);
        }
        return msgs;
    }
}
```
