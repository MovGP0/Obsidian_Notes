Forms are a special kind of [[Microsoft Bot Builder Dialogs]], representing a series of questions to answer.

## PromtAttribute

- `{&}` is replaced with property name
- `{||}` is replaced with possible enum values

## Declarative Form

```csharp
internal static IDialog<Appointment> MakeRootDialog()
{
	return Chain.From(() => FormDialog.FromForm(Appointment.BuildForm));
}

[Serializable]
public class Appointment
{
	[Prompt("When would you like to book your {&}?")]
	public DateTime AppointmentDate { get; set; }
	
	[Prompt("What is the {&}")]
	public string PatientName { get; set; }
	
	[Prompt("What are the {&} you are looking for? {||}")]
	public Specialty? Specialties;
	
	[Prompt("Any {&} to the Doctor?")]
	public string SpecialInstructions { get; set; }
	
	public static IForm<Appointment> BuildForm()
	{
	    OnCompletionAsyncDelegate<Appointment> processAppointment =
		    async (context, state) =>
		    {
			    IMessageActivity reply = context.MakeMessage();
	            reply.Text = $"We are confirming your appointment for {state.PatientName} at {state.AppointmentDate.ToShortTimeString()}." +
	            " Reference ID: " + Guid.NewGuid().ToString().Substring(0, 5);
	    // Save State to database here...
	    await context.PostAsync(reply); };
	    
	    return new FormBuilder<Appointment>()
		    .Message("Welcome!")
			.OnCompletion(processAppointment)
			.Build();
	}
};
```

## Using FormBuilder

```csharp
public static IForm<FormFlowSimple> BuildForm()
{
	[Prompt("Please enter your Credit card Number in international format.")]
	public string CreditCardNumber;

	[Template(TemplateUsage.EnumSelectOne, "What is your preferred operating system? {||}")]
	public OperatingSystemOptions? OS;

    // other properties

	return new FormBuilder<FormFlowSimple>()
		.Message("Welcome!")
		.Field(nameof(Cam))
		.Field(nameof(Cpu))
		.Field(nameof(Drive))
		.Field(nameof(Screen))
		.Field(nameof(OS))
		.Field(nameof(Delivery))
		.Field(nameof(CreditCardNumber), IsCreditCard)
		.Build();
	}
	
	private static bool IsCreditCard(FormFlowSimple state)
	{
		return state.delivery == Deliveryoptions.CreditCard;
	}
```

