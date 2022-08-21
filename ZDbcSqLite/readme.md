Patched to ZDBSqlite.pas - to enable encrytion/decryption using wxSqlite3mc ('wx')
******************************************************************************
            ZDBSqlite.pas -  for Zeoslib merged revision(s) 7863
            ZDBSqlite2.pas -- for older version of Zeoslib

Many thanks to marsupilam (https://github.com/marsupilami79/zeoslib) guidance in solving my problem. This patch would not 
be possible without the advice from master marsupilam.

I have added a few lines in the procedure TZSQLiteConnection.Open of the original ZDBSlite.pas (zeoslib7.2.14) file.

1. To access an encrypted sqlite database, assign the cipher and password properties before you open the connection, as follows:

      zConnection1.Properties.Values['Cipher'] := 'aes256cbc';                           
      // Values['Cipher'] can be: 'chacha20', 'aes128cbc', 'aes256cbc', 'sqlcipher' or 'rc4'        
      zConnection1.Password := 'Password';        
      zConnection1.Connect;
      
   {for SEE (not wx): 
        leave Values['cipher'] to blank   
        set password to [cipher]:[passphrase] eg aes128:Mykey
        you can use other password format, set zConnection1.Properties.Values['keyfmt'] to 'hexkey' or 'textkey' }         
      
  
2. To change Cipher (after step 1 above):  
                         
      ExecuteDirect('PRAGMA cipher=' +QuotedStr('chacha20'));                           
      // You can replace 'chacha20' with 'aes256cbc', 'aes128cbc', 'sqlcipher' or 'rc4' as desired

   {for SEE, please refer to documentation. eg 'PRAGMA rekey 'rc4:Mykey', 'aes256:Mykey'} 

  
3. To change password (after step 1 above):  
                         
      ZConnection1.ExecuteDirect('PRAGMA rekey =' + QuotedStr(pw));                           
      //pw is a string variable containing you new password    

   {For SEE, please refer to documentation. eg 'PRAGMA 'myNewKey'}
           

***********************************************************************************
