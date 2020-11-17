The Audit API provides access to mortgage origination data useful for quality control. You can use the Audit API to verify the source of data used in underwriting, compliance with regulatory requirements, and to completely re-underwrite a mortgage with fresh data from the source. Here are some examples of what you can do with `q.audit`:

## Verify income used during mortgage underwriting

```
const income_verification = await q.audit.verify({
  loan_id: 'd82h99bb',
  verify: 'income'
});

print(income_verification);
```
```
Loan ID: d82h99bb
Income: $155,200
Sources: 
  - Name: IRS Transcript 2019
    Collected: 2020-05-17 12:56:21.281
    Wages Salaries Tips: 155200
```

## Verify compliance with TILA closing window regulation

```
const closing_verification = await q.audit.verify({
  loan_id: 'd82h99bb',
  verify: 'closing'
});

const closing_window = closing_verification['notarization']['time'] - closing_verification['closing_disclosure']['receipt_time'];
const closing_window_in_days = difference_in_business_days(closing_window);

if ( closing_window_in_days < 7 ) {
  throw new tila_violation("Time between Closing Disclosure recipt and mortgage notarization is less than the 7 business day requirement!");
}
```

## Re-underwrite a mortgage with re-verified income, employment, and credit

```
const reunderwrite_result = await q.audit.reunderwrite({
  loan_id: 'd82h99bb',
  reunderwrite: ['income', 'employment', 'credit']
})

print(reunderwrite_result);
```
```
Loan ID: d82h99bb
Underwriting Rules ID: aa92sjw2
Underwriting Results:
	Original Result: Passed
    Current Result: Passed
    Original Time: 2020-05-17 14:01:16.283
    Current TIme: 2020-06-21 15:21:49.266
Income:
	Original:
    	Source: Equifax
        Employer: XYZ Industries
        Income: 155200
        Status: Employed
        As of: 2020-05-17 14:01:15.121
    Current:
    	Source: Equifax
        Employer: XYZ Industries
        Income: 155200
        Status: Employed
        As of: 2020-06-21 15:21:48.110
Credit:
	Original:
    	Source: CoreLogic Trimerge Report
        FICO: [748, 744, 752]
      	As of: 2020-05-17 14:01:14.998
    Current:
    	Source: CoreLogic Trimerge Report
        FICO: [750, 744, 754]
      	As of: 2020-06-21 15:21:48.200
```