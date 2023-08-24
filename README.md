import re
from Bio.Seq import Seq
from Bio.SeqUtils import molecular_weight

def calculate_statistics(sequence):
    length = len(sequence)
    gc_content = (sequence.count("G") + sequence.count("C")) / length * 100
    at_content = (sequence.count("A") + sequence.count("T")) / length * 100
    weight = molecular_weight(Seq(sequence))
    return length, gc_content, at_content, weight

def transcription(dna_sequence):
    return dna_sequence.replace("T", "U")

def translation(rna_sequence, reading_frame=0):
    rna_seq = Seq(rna_sequence)
    protein_seq = rna_seq[reading_frame:].translate()
    return str(protein_seq)

def motif_search(sequence, motif):
    matches = [match.start() for match in re.finditer(motif, sequence)]
    return matches

def reverse_complement(dna_sequence):
    return str(Seq(dna_sequence).reverse_complement())

if __name__ == "__main__":
    sequence = input("Enter a DNA/RNA/Protein sequence: ")

    while True:
        print("\nChoose an option:")
        print("1. Calculate basic statistics")
        print("2. Transcription")
        print("3. Translation")
        print("4. Motif search")
        print("5. Reverse complement")
        print("6. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            length, gc_content, at_content, weight = calculate_statistics(sequence)
            print(f"Sequence Length: {length}")
            print(f"GC Content: {gc_content:.2f}%")
            print(f"AT Content: {at_content:.2f}%")
            print(f"Molecular Weight: {weight:.2f} g/mol")

        elif choice == "2":
            if "T" in sequence:  # Assuming DNA sequence
                rna_seq = transcription(sequence)
                print(f"Transcribed RNA Sequence: {rna_seq}")
            else:
                print("Input sequence is not DNA.")

        elif choice == "3":
            if "U" in sequence:  # Assuming RNA sequence
                reading_frame = int(input("Enter reading frame (0, 1, or 2): "))
                protein_seq = translation(sequence, reading_frame)
                print(f"Translated Protein Sequence: {protein_seq}")
            else:
                print("Input sequence is not RNA.")

        elif choice == "4":
            motif = input("Enter motif to search: ")
            matches = motif_search(sequence, motif)
            if matches:
                print(f"Motif '{motif}' found at positions: {', '.join(map(str, matches))}")
            else:
                print(f"Motif '{motif}' not found in the sequence.")

        elif choice == "5":
            if "T" in sequence:  # Assuming DNA sequence
                rev_comp = reverse_complement(sequence)
                print(f"Reverse Complement Sequence: {rev_comp}")
            else:
                print("Input sequence is not DNA.")

        elif choice == "6":
            print("Exiting the program.")
            break

        else:
            print("Invalid choice. Please enter a valid option.")

   
