package bank.management.controller;

import java.util.Optional;

import bank.management.dto.Account;
import bank.management.service.BankManagementService;
import bank.management.view.EndView;
import bank.management.view.FailView;



public class BankManagementController {
	
	BankManagementService service = BankManagementService.getInstance();
	//전체계좌
	public void allAccountGet() {
		EndView.allAccountView(service.getAllAccount());
		
	}
	
	// 하나 검색
	public void accountSearch(String Id) {
		try {
			EndView.accountView((service.searchAccount(Id)));
		} catch (Exception e) {
			FailView.failMessageView(e.getMessage());
		}
	}
	// 잔액확인
	public void balanceGet(String Id) {
		try {
			EndView.balanceView(service.getBalance(Id));
		} catch (Exception e) {
			FailView.failMessageView("존재하지 않는 계좌");
		}
	}
	
	//입금
	public void moneyIN(String Id, int money) {
		try {
			money = service.inMoney(Id, money);
			EndView.MessageView("입금");
			balanceGet(Id);
		} catch (Exception e) {
			FailView.failMessageView(e.getMessage());
		}
	}
	
	//출금
	public void moneyGet(String Id, int money) {
		try {
			money = service.getMoney(Id, money);
			FailView.failMessageView("존재하지 않는 계좌");
			balanceGet(Id);
		} catch (Exception e) {
			EndView.MessageView("출금");
		}
		if (money == -1) {
			FailView.failMessageView("잔액 부족");
		} else {
			
		}
	}
	
	//계좌생성
	public void accountInsert (Account newAccount) {
		EndView.MessageView(service.insertAccount(newAccount));
	}
	
	//계좌 삭제
	public void deleteAccount(String Id, String residentNum) {
		try {
			Account account = service.deleteAccount(Id, residentNum);
			if (Optional.ofNullable(account).isPresent()) {
				EndView.MessageView("계좌 삭제");
			} else {
				FailView.failMessageView("주민등록번호가 일치하지 않음");
			}
		} catch (Exception e) {
			FailView.failMessageView("존재하지 않는 Id");
		}
	}
	
	
	
	
	
}
