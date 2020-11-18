The Audit API allows you to review activity performed by Q. Here are some examples of what you can do with `q.audit`:
<br><br>
## Review a customer
Review a customer's KYC results for AML compliance
<pre style="background-color: #EEEEEE;"  >
<span style="color:#00AA00">data</span> = q.audit.<span style="color:#DD0000">review</span>({
  customer: 'z82j2i1a',
  items: <span style="color:#DD0000">'kyc'</span>
});

print(<span style="color:#00AA00">data</span>);
</pre>
<br>
output:
<pre style="background-color: #EEEEEE;"  >
id: state driver license
selfie photo: validated
ssn: validated
ofac: not present
pep: not present
last reviewed: 11-03-20
</pre>
<br><br>
## Review underwriting
Review the data used for a borrower income calculation
<pre style="background-color: #EEEEEE;"  >
<span style="color:#00AA00">data</span> = q.audit.<span style="color:#DD0000">review</span>({
  loan: 'd82h99bb',
  items: <span style="color:#DD0000">'income'</span>
});

print(<span style="color:#00AA00">data</span>);
</pre>
<br>
output:
<pre style="background-color: #EEEEEE;"  >
annual: 155200
data sources:
  - name: IRS Transcript 2019
    retrieved: 2020-05-17 12:56:21.281
    wages salaries tips: 155200
</pre>
<br><br>
## Review closing
Review a mortgage closing disclosure window to check for regulatory issues on closed loans
<pre style="background-color: #EEEEEE;"  >
<span style="color:#00AA00">data</span> = q.audit.<span style="color:#DD0000">review</span>({
  loan: 'd82h99bb',
  items: <span style="color:#DD0000">'closing'</span>
});

closing_time = <span style="color:#00AA00">data</span>['closing']['time'];
closing_disclosure_time = <span style="color:#00AA00">data</span>['closing_disclosure']['time'];

print(calculate_business_days( closing_time - closing_disclosure_time ));
</pre>
<br>
output:
<pre style="background-color: #EEEEEE;"  >
10.23
</pre>
<br><br>
## Re-underwrite a mortgage
Re-underwrite a closed mortgage for quality control. Pulls updated data from original sources, and checks against original underwriting criteria
<pre style="background-color: #EEEEEE;"  >
<span style="color:#00AA00">data</span> = q.audit.<span style="color:#DD0000">reunderwrite</span>({
  loan: 'd82h99bb',
  items: <span style="color:#DD0000">['income', 'credit']</span>
});

print(<span style="color:#00AA00">data</span>);
</pre>
<br>
output:
<pre style="background-color: #EEEEEE;"  >
new result: passed
original result: passed
new items:
  income:
    source: Equifax
    employer: XYZ Industries
    income: 155200
  credit:
    source: CoreLogic Trimerge Report
    fico: [750, 744, 754]
</pre>