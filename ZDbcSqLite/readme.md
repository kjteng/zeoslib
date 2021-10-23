Patched to ZDBSqlite.pas - to enable encrytion/decryption using wxSqlite3mc
******************************************************************************

Many thanks to marsupilam (https://github.com/marsupilami79/zeoslib) guidance in solving my problem. This patch would not 
be possible without the advice from master marsupilam.

I have added a few lines to the original ZDBSlite.pas (zeoslib7.2.14) file i.e. lines 360-368

1. To access an encrypted sqlite database:
   Assign the cipher and password properties before you open the connection, as follows:
      zConnection1.Properties.Values['Cipher'] := 'aes256cbc';  
      zConnection1.Password := 'Password';
      zConnection1.Connect;
      
  
2. To change Cipher (after step 1 above):
     ExecuteDirect('PRAGMA cipher=' +QuotedStr('chacha20'));
   You can replace 'chacha20' with 'aes256cbc', 'aes128cbc', 'sqlcipher' or 'rc4' as desired
  
  
3. To changing password (after step 1 above):
      ZConnection1.ExecuteDirect('PRAGMA rekey =' + QuotedStr(pw)); //pw is a string variable containing you new password    
     
           

***********************************************************************************
