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
