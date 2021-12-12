Patched to ZDBSqlite.pas - to enable encrytion/decryption using wxSqlite3mc ('wx')
******************************************************************************

Many thanks to marsupilam (https://github.com/marsupilami79/zeoslib) guidance in solving my problem. This patch would not 
be possible without the advice from master marsupilam.

I have added a few lines to the original ZDBSlite.pas (zeoslib7.2.14) file i.e. lines 360-368

1. To access an encrypted sqlite database:
   Assign the cipher and password properties before you open the connection, as follows:
      zConnection1.Properties.Values['Cipher'] := 'aes256cbc';  //if you are using SEE (not wx), leave Values['cipher'] to blank
      zConnection1.Password := 'Password';                     //if you are using SEE (not wx), set password to [cipher]:[passphrase] eg aes128:Mykey
      zConnection1.Connect;
      
  
2. To change Cipher (after step 1 above):
     ExecuteDirect('PRAGMA cipher=' +QuotedStr('chacha20'));  // For SEE, please refer to documentation. eg 'PRAGMA rekey 'rc4:Mykey', 'aes256:Mykey'  
   You can replace 'chacha20' with 'aes256cbc', 'aes128cbc', 'sqlcipher' or 'rc4' as desired
  
  
3. To change password (after step 1 above):
      ZConnection1.ExecuteDirect('PRAGMA rekey =' + QuotedStr(pw)); //pw is a string variable containing you new password    
      // For SEE, please refer to documentation. eg 'PRAGMA 'myNewKey'                                                                                   
           

***********************************************************************************
