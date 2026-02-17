# Low-Level-Soliditiy

contract Sender {

    receive() external payable {}

    function withdrawTransfer(address payable _to) public {
        _to.transfer(10);
    }

    function withdrawSend(address payable _to) public {
       bool isSent = _to.send(10);

       require(isSent, "Sending the funds was unsuccessful");
    }

}
contract ReceiverNoAction {

    function balance() public view returns(uint) {
        return address(this).balance;
    }
    receive() external payable {}
}
contract ReceiverAction {
    
    uint public balanceReceived;

    function balance() public view returns(uint) {
        return address(this).balance;
    }
}

# LOW DEPTH

// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.7.0 <0.9.0; 
contract ContractOne {

    mapping (address => uint) public addressBalance;

    function deposit() public payable {
        addressBalance[msg.sender] += msg.value;
    }
}

contract ContractTwo {
    receive() external payable {}

    function depositonContractOne(address _contractOne) public {
        ContractOne one = ContractOne(_contractOne);
        one.deposit{value: 10,gas: 100000}();
    }
}
# WithSignature
// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.7.0 <0.9.0; 
contract ContractOne {

    mapping (address => uint) public addressBalance;

    function deposit() public payable {
        addressBalance[msg.sender] += msg.value;
    }

     
}

contract ContractTwo {
    receive() external payable {}

    function depositonContractOne(address _contractOne) public {
        bytes memory payload = abi.encodeWithSignature("deposit()");
       (bool success,) =_contractOne.call{value: 10,gas: 100000}(payload); 
       require(success);

    }
}     

# WITHOUT PAYLOAD
// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.7.0 <0.9.0; 
contract ContractOne {

    mapping (address => uint) public addressBalance;

    function deposit() public payable {
        addressBalance[msg.sender] += msg.value;
    }

     receive() external payable { 
        deposit();
     } 
}

contract ContractTwo {
    receive() external payable {}

    function depositonContractOne(address _contractOne) public {
       (bool success,) =_contractOne.call{value: 10,gas: 100000}(""); 
       require(success);

    }
}    
