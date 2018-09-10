# 123
namespace Counter
{
    class Program
    {
        static void Main(string[] args)
        {
            var input = args[0];
            var result = args[1];
			
		
            var text = File.ReadAllText(input, Encoding.UTF8);
            
		
			var pattern = @"[^a-z-]+";
            var regex = new Regex(pattern, RegexOptions.IgnoreCase);

			
            var array = regex.Split(text.ToLower()).Where(e => e.Length >= 1);
			
		
            var query = array.GroupBy(e => e)
                .Select(e => new {Word = e.Key, Count = e.Count()}) 
                .OrderBy(e => e.Word[0])
                .ThenByDescending(e => e.Count)
                .GroupBy(e => e.Word[0]);

          var builder = new StringBuilder();
            
            foreach (var wordsCount in query)
            {
                builder.Append(wordsCount.Key).AppendLine();
                foreach (var wordCount in wordsCount)
                {
                    builder.Append($"{wordCount.Word} {wordCount.Count}").AppendLine();
                }
            }

            File.WriteAllText(args[1], builder.ToString(), Encoding.UTF8);
            
            Console.ReadKey();
        }
    }
}
