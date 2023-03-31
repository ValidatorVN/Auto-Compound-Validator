# Auto-Compound-Validator

# AutoRestake Validator

Lưu ý trước khi cài đặt, kiểm tra các biến số:

    <Binary>
    <valoperadd>
    <Chain>
    <Denom>
    password = là pass lúc tạo ví chạy node, yêu cầu nhập để thực thi giao dịch, không phải private key.
    

Tạo 1 file *.sh

    nano autostake.sh

Script:

    #!/usr/bin/expect -f

    while {1} {
    # Run the first command
    spawn <Binary> tx distribution withdraw-rewards <valoperadd> --commission --from wallet --chain-id <Chain> --gas-prices 0.1<Denom> --gas-adjustment 1.5 --gas auto -y

    # Expect the passphrase prompt and send the password
    expect "Enter keyring passphrase:"
    send "password\r"

    # Wait for the first command to finish
    expect eof

    # Wait for 5 seconds
    sleep 5

    # Run the second command
    spawn <Binary> tx staking delegate <valoperadd> 1000000<Denom> --from wallet --chain-id <Chain> --gas-prices 0.1<Denom> --gas-adjustment 1.5 --gas auto -y

    # Expect the passphrase prompt and send the password
    expect "Enter keyring passphrase:"
    send "password\r"

    # Wait for the second command to finish
    expect eof

    # Wait for 300 seconds before running the commands again
    sleep 300
    }
  
Lưu script lại, sau đó cấp quyền:
  
    chmox +x *.sh
      
Mở 1 cửa sổ tmux, sau đó chạy script:
  
    tmux
      
    ./autostake.sh
      
 Như vậy, khi lệnh script chạy sẽ tự động claim rewards và commisson, sau đó 5 giây sẽ stake lại vào validator của bạn, với số lượng tuỳ bạn set bao nhiêu.
 Nên treo bằng tmux để scipt chạy liên tục, hoặc dùng crontab -e. 
 
    Chanel: https://t.me/RunnodeVietNamese
    Youtube: https://www.youtube.com/@nodevalidatorvietnam
    Twitter: https://twitter.com/NodeValidatorVN
