package ewicket.editors;


import java.io.StringWriter;
import java.text.Collator;
import java.util.ArrayList;
import java.util.Collections;
import java.util.StringTokenizer;

import org.eclipse.core.resources.IFile;
import org.eclipse.core.resources.IMarker;
import org.eclipse.core.resources.IResourceChangeEvent;
import org.eclipse.core.resources.IResourceChangeListener;
import org.eclipse.core.resources.ResourcesPlugin;
import org.eclipse.core.runtime.IProgressMonitor;
import org.eclipse.jdt.internal.ui.javaeditor.CompilationUnitEditor;
import org.eclipse.jface.dialogs.ErrorDialog;
import org.eclipse.swt.widgets.Display;
import org.eclipse.ui.IEditorInput;
import org.eclipse.ui.IEditorPart;
import org.eclipse.ui.IEditorSite;
import org.eclipse.ui.IFileEditorInput;
import org.eclipse.ui.IWorkbenchPage;
import org.eclipse.ui.PartInitException;
import org.eclipse.ui.editors.text.TextEditor;
import org.eclipse.ui.ide.IDE;
import org.eclipse.ui.part.FileEditorInput;
import org.eclipse.ui.part.MultiPageEditorPart;
import org.eclipse.wst.sse.ui.StructuredTextEditor;

/**
 * 
 * @author Francesco De Simone
 *
 */
@SuppressWarnings("restriction")
public class eWicketEditor extends MultiPageEditorPart implements IResourceChangeListener{


	private CompilationUnitEditor javaEditor;
	

	private StructuredTextEditor htmlEditor;


	private TextEditor propEditor;
	
	
	
	public eWicketEditor() {
		super();
		ResourcesPlugin.getWorkspace().addResourceChangeListener(this);
		
	}
	
	
	void createPage0() {
		try {
			
			javaEditor = new CompilationUnitEditor() ;
			
			int index = addPage(javaEditor, getEditorInput());
			setPageText(index, javaEditor.getTitle());
			setPageText(index, "JAVA");
		} catch (PartInitException e) {
			ErrorDialog.openError(
				getSite().getShell(),
				"Error creating nested text editor",
				null,
				e.getStatus());
		}
	}
	
	
	void createPage1() {

		try {
		
			htmlEditor = new StructuredTextEditor();
			
			String path = ((FileEditorInput)javaEditor.getEditorInput()).getFile().getProjectRelativePath().toString();
			
			IFile htmlFile = ((FileEditorInput)javaEditor.getEditorInput()).getFile().getProject().getFile(path.replace(".java", ".html"));
			
			FileEditorInput editor = new FileEditorInput(htmlFile);
		
		
			
			int index = addPage(htmlEditor, editor);
			setPageText(index, javaEditor.getTitle());
			setPageText(index, "HTML");
		} catch (PartInitException e) {
			ErrorDialog.openError(
				getSite().getShell(),
				"Error creating nested text editor",
				null,
				e.getStatus());
		}

	
	}
	
	
	
	void createPage2() {
		try {
			propEditor = new TextEditor();
			
			String path = ((FileEditorInput)javaEditor.getEditorInput()).getFile().getProjectRelativePath().toString();
			
			IFile htmlFile = ((FileEditorInput)javaEditor.getEditorInput()).getFile().getProject().getFile(path.replace(".java", ".properties"));
				
			FileEditorInput editor = new FileEditorInput(htmlFile);
							
			
			int index = addPage(propEditor, editor);
			setPageText(index, propEditor.getTitle());
			setPageText(index, "Properties");
		} catch (PartInitException e) {
			ErrorDialog.openError(
				getSite().getShell(),
				"Error creating nested text editor",
				null,
				e.getStatus());
		}
	}
	
	
	protected void createPages() {
		createPage0();
		createPage1();
		createPage2();
	}
	
	
	public void dispose() {
		ResourcesPlugin.getWorkspace().removeResourceChangeListener(this);
		super.dispose();
	}
	/**
	 * Save all file 
	 * TODO change this with configuration by user
	 */
	public void doSave(IProgressMonitor monitor) {
		getEditor(0).doSave(monitor);
		getEditor(1).doSave(monitor);
		getEditor(2).doSave(monitor);
	}
	
	/**
	 * Save as current file only 
	 * TODO change this with configuration by user
	 */
	
	public void doSaveAs() {
		IEditorPart editor = getEditor(0);
		editor.doSaveAs();
		setPageText(0, editor.getTitle());
		setInput(editor.getEditorInput());
	}
	
	
	public void gotoMarker(IMarker marker) {
		setActivePage(0);
		IDE.gotoMarker(getEditor(0), marker);
	}
	
	
	public void init(IEditorSite site, IEditorInput editorInput)
		throws PartInitException {
		
		
		if (!(editorInput instanceof IFileEditorInput))
			throw new PartInitException("Invalid Input: Must be IFileEditorInput");
		super.init(site, editorInput);
	}
	
	
	public boolean isSaveAsAllowed() {
		return true;
	}
	
	
	protected void pageChange(int newPageIndex) {
		super.pageChange(newPageIndex);
		if (newPageIndex == 2) {
			sortWords();
		}
	}
	/**
	 * Closes all project files on project close.
	 */
	public void resourceChanged(final IResourceChangeEvent event){
		if(event.getType() == IResourceChangeEvent.PRE_CLOSE){
			Display.getDefault().asyncExec(new Runnable(){
				public void run(){
					IWorkbenchPage[] pages = getSite().getWorkbenchWindow().getPages();
					for (int i = 0; i<pages.length; i++){
						
						
						if(((FileEditorInput)javaEditor.getEditorInput()).getFile().getProject().equals(event.getResource())){
							IEditorPart editorPart = pages[i].findEditor(javaEditor.getEditorInput());
							pages[i].closeEditor(editorPart,true);
						}
					}
				}            
			});
		}
	}

	@SuppressWarnings("unchecked")
	void sortWords() {

		String editorText =
			javaEditor.getDocumentProvider().getDocument(javaEditor.getEditorInput()).get();

		StringTokenizer tokenizer =
			new StringTokenizer(editorText, " \t\n\r\f!@#\u0024%^&*()-_=+`~[]{};:'\",.<>/?|\\");
		@SuppressWarnings("rawtypes")
		ArrayList editorWords = new ArrayList();
		while (tokenizer.hasMoreTokens()) {
			editorWords.add(tokenizer.nextToken());
		}

		Collections.sort(editorWords, Collator.getInstance());
		StringWriter displayText = new StringWriter();
		for (int i = 0; i < editorWords.size(); i++) {
			displayText.write(((String) editorWords.get(i)));
			displayText.write(System.getProperty("line.separator"));
		}
	}
}
